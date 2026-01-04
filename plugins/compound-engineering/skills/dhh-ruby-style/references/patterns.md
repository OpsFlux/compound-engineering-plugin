# DHH Ruby/Rails 模式参考

从 37signals 的 Campfire 代码库和 DHH 的公共教学中提取的综合代码模式。

## 控制器模式

### REST-纯控制器设计

DHH 的控制器哲学对于 REST 来说是“原教旨主义”的。每个控制器仅通过 7 个标准操作映射到一个资源。

```ruby
# ✅ CORRECT: Standard REST actions only
class MessagesController < ApplicationController
  def index; end
  def show; end
  def new; end
  def create; end
  def edit; end
  def update; end
  def destroy; end
end

# ❌ WRONG: Custom actions
class MessagesController < ApplicationController
  def archive    # NO
  def unarchive  # NO
  def search     # NO
  def drafts     # NO
end

# ✅ CORRECT: New controllers for custom behavior
class Messages::ArchivesController < ApplicationController
  def create  # archives a message
  def destroy # unarchives a message
end

class Messages::DraftsController < ApplicationController
  def index   # lists drafts
end

class Messages::SearchesController < ApplicationController
  def show    # shows search results
end
```


### 控制器对共享行为的关注

```ruby
# app/controllers/concerns/room_scoped.rb
module RoomScoped
  extend ActiveSupport::Concern

  included do
    before_action :set_room
  end

  private
    def set_room
      @room = Current.user.rooms.find(params[:room_id])
    end
end

# Usage
class MessagesController < ApplicationController
  include RoomScoped
end
```


### 完整的控制器示例

```ruby
class MessagesController < ApplicationController
  include ActiveStorage::SetCurrent, RoomScoped

  before_action :set_room, except: :create
  before_action :set_message, only: %i[ show edit update destroy ]
  before_action :ensure_can_administer, only: %i[ edit update destroy ]

  layout false, only: :index

  def index
    @messages = find_paged_messages
    if @messages.any?
      fresh_when @messages
    else
      head :no_content
    end
  end

  def create
    set_room
    @message = @room.messages.create_with_attachment!(message_params)
    @message.broadcast_create
    deliver_webhooks_to_bots
  rescue ActiveRecord::RecordNotFound
    render action: :room_not_found
  end

  def show
  end

  def edit
  end

  def update
    @message.update!(message_params)
    @message.broadcast_replace_to @room, :messages,
      target: [ @message, :presentation ],
      partial: "messages/presentation",
      attributes: { maintain_scroll: true }
    redirect_to room_message_url(@room, @message)
  end

  def destroy
    @message.destroy
    @message.broadcast_remove_to @room, :messages
  end

  private
    def set_message
      @message = @room.messages.find(params[:id])
    end

    def ensure_can_administer
      head :forbidden unless Current.user.can_administer?(@message)
    end

    def find_paged_messages
      case
      when params[:before].present?
        @room.messages.with_creator.page_before(@room.messages.find(params[:before]))
      when params[:after].present?
        @room.messages.with_creator.page_after(@room.messages.find(params[:after]))
      else
        @room.messages.with_creator.last_page
      end
    end

    def message_params
      params.require(:message).permit(:body, :attachment, :client_message_id)
    end

    def deliver_webhooks_to_bots
      bots_eligible_for_webhook.excluding(@message.creator).each { |bot| bot.deliver_webhook_later(@message) }
    end

    def bots_eligible_for_webhook
      @room.direct? ? @room.users.active_bots : @message.mentionees.active_bots
    end
end
```


## 模型模式

### 语义关联命名

```ruby
class Message < ApplicationRecord
  # ✅ Semantic names that express domain concepts
  belongs_to :creator, class_name: "User"
  belongs_to :room
  has_many :mentions
  has_many :mentionees, through: :mentions, source: :user

  # ❌ Generic names
  belongs_to :user  # Too generic - creator is clearer
end

class Room < ApplicationRecord
  has_many :memberships
  has_many :users, through: :memberships
  has_many :messages, dependent: :destroy

  # Semantic scope
  scope :direct, -> { where(direct: true) }

  def direct?
    direct
  end
end
```


### 范围设计

```ruby
class Message < ApplicationRecord
  # Eager loading scopes
  scope :with_creator, -> { includes(:creator) }
  scope :with_attachments, -> { includes(attachment_attachment: :blob) }

  # Cursor-based pagination scopes
  scope :page_before, ->(cursor) {
    where("id < ?", cursor.id).order(id: :desc).limit(50)
  }
  scope :page_after, ->(cursor) {
    where("id > ?", cursor.id).order(id: :asc).limit(50)
  }
  scope :last_page, -> { order(id: :desc).limit(50) }

  # Status scopes as chainable lambdas
  scope :recent, -> { where("created_at > ?", 24.hours.ago) }
  scope :pinned, -> { where(pinned: true) }
end
```


### 自定义创建方法

```ruby
class Message < ApplicationRecord
  def self.create_with_attachment!(params)
    transaction do
      message = create!(params.except(:attachment))
      message.attach_file(params[:attachment]) if params[:attachment].present?
      message
    end
  end

  def attach_file(attachment)
    file.attach(attachment)
    update!(has_attachment: true)
  end
end
```


### 模型授权

```ruby
class User < ApplicationRecord
  def can_administer?(message)
    message.creator == self || admin?
  end

  def can_access?(room)
    rooms.include?(room) || admin?
  end

  def can_invite_to?(room)
    room.creator == self || admin?
  end
end

# Usage in controller
def ensure_can_administer
  head :forbidden unless Current.user.can_administer?(@message)
end
```


### 模特广播

```ruby
class Message < ApplicationRecord
  after_create_commit :broadcast_create
  after_update_commit :broadcast_update
  after_destroy_commit :broadcast_destroy

  def broadcast_create
    broadcast_append_to room, :messages,
      target: "messages",
      partial: "messages/message"
  end

  def broadcast_update
    broadcast_replace_to room, :messages,
      target: dom_id(self, :presentation),
      partial: "messages/presentation"
  end

  def broadcast_destroy
    broadcast_remove_to room, :messages
  end
end
```


## 当前属性模式

### 定义

```ruby
# app/models/current.rb
class Current < ActiveSupport::CurrentAttributes
  attribute :user
  attribute :session
  attribute :request_id
  attribute :user_agent

  resets { Time.zone = nil }

  def user=(user)
    super
    Time.zone = user&.time_zone
  end
end
```


### 控制器中的设置

```ruby
class ApplicationController < ActionController::Base
  before_action :set_current_attributes

  private
    def set_current_attributes
      Current.user = authenticate_user
      Current.session = session
      Current.request_id = request.request_id
      Current.user_agent = request.user_agent
    end
end
```


### 整个应用程序的使用

```ruby
# In models
class Message < ApplicationRecord
  before_create :set_creator

  private
    def set_creator
      self.creator ||= Current.user
    end
end

# In views
<%= Current.user.name %>

# In jobs
class NotificationJob < ApplicationJob
  def perform(message)
    # Current is reset in jobs - pass what you need
    message.room.users.each { |user| notify(user, message) }
  end
end
```


## 红宝石习语

### 嵌套条件的保护子句

```ruby
# ✅ Guard clauses
def process_message
  return unless message.valid?
  return if message.spam?
  return unless Current.user.can_access?(message.room)

  message.deliver
end

# ❌ Nested conditionals
def process_message
  if message.valid?
    unless message.spam?
      if Current.user.can_access?(message.room)
        message.deliver
      end
    end
  end
end
```


### 无表达式 Case 语句

```ruby
# ✅ Clean case without expression
def status_class
  case
  when urgent? then "bg-red"
  when pending? then "bg-yellow"
  when completed? then "bg-green"
  else "bg-gray"
  end
end

# For routing/dispatch logic
def find_paged_messages
  case
  when params[:before].present?
    messages.page_before(params[:before])
  when params[:after].present?
    messages.page_after(params[:after])
  else
    messages.last_page
  end
end
```


### 方法链接

```ruby
# ✅ Fluent, chainable API
@room.messages
     .with_creator
     .with_attachments
     .excluding(@message.creator)
     .page_before(cursor)

# On collections
bots_eligible_for_webhook
  .excluding(@message.creator)
  .each { |bot| bot.deliver_webhook_later(@message) }
```


### 隐式返回

```ruby
# ✅ Implicit return - the Ruby way
def full_name
  "#{first_name} #{last_name}"
end

def can_administer?(message)
  message.creator == self || admin?
end

# ❌ Explicit return (only when needed for early exit)
def full_name
  return "#{first_name} #{last_name}"  # Unnecessary
end
```


## 查看模式

### 复杂 HTML 的辅助方法

```ruby
# app/helpers/messages_helper.rb
module MessagesHelper
  def message_container(message, &block)
    tag.div(
      id: dom_id(message),
      class: message_classes(message),
      data: {
        controller: "message",
        message_id_value: message.id,
        action: "click->message#select"
      },
      &block
    )
  end

  private
    def message_classes(message)
      classes = ["message"]
      classes << "message--mine" if message.creator == Current.user
      classes << "message--highlighted" if message.highlighted?
      classes.join(" ")
    end
end
```


### Turbo 框架模式

```erb
<%# app/views/messages/index.html.erb %>
<%= turbo_frame_tag "messages", data: { turbo_action: "advance" } do %>
  <%= render @messages %>

  <% if @messages.any? %>
    <%= link_to "Load more",
          room_messages_path(@room, before: @messages.last.id),
          data: { turbo_frame: "messages" } %>
  <% end %>
<% end %>
```


### 刺激控制器集成

```erb
<div data-controller="message-form"
     data-message-form-submit-url-value="<%= room_messages_path(@room) %>">
  <%= form_with model: [@room, Message.new],
        data: { action: "submit->message-form#submit" } do |f| %>
    <%= f.text_area :body,
          data: { action: "keydown.enter->message-form#submitOnEnter" } %>
    <%= f.submit "Send" %>
  <% end %>
</div>
```


## 测试模式

### 首先进行系统测试

```ruby
# test/system/messages_test.rb
class MessagesTest < ApplicationSystemTestCase
  test "sending a message" do
    sign_in users(:david)
    visit room_path(rooms(:watercooler))

    fill_in "Message", with: "Hello, world!"
    click_button "Send"

    assert_text "Hello, world!"
  end

  test "editing own message" do
    sign_in users(:david)
    visit room_path(rooms(:watercooler))

    within "#message_#{messages(:greeting).id}" do
      click_on "Edit"
    end

    fill_in "Message", with: "Updated message"
    click_button "Save"

    assert_text "Updated message"
  end
end
```


### 工厂固定装置

```yaml
# test/fixtures/users.yml
david:
  name: David
  email: david@example.com
  admin: true

jason:
  name: Jason
  email: jason@example.com
  admin: false

# test/fixtures/rooms.yml
watercooler:
  name: Water Cooler
  creator: david
  direct: false

# test/fixtures/messages.yml
greeting:
  body: Hello everyone!
  room: watercooler
  creator: david
```


### API 集成测试

```ruby
# test/integration/messages_api_test.rb
class MessagesApiTest < ActionDispatch::IntegrationTest
  test "creating a message via API" do
    post room_messages_url(rooms(:watercooler)),
      params: { message: { body: "API message" } },
      headers: auth_headers(users(:david))

    assert_response :success
    assert Message.exists?(body: "API message")
  end
end
```


## 配置模式

### 固定队列设置

```ruby
# config/queue.yml
default: &default
  dispatchers:
    - polling_interval: 1
      batch_size: 500
  workers:
    - queues: "*"
      threads: 5
      processes: 1
      polling_interval: 0.1

development:
  <<: *default

production:
  <<: *default
  workers:
    - queues: "*"
      threads: 10
      processes: 2
```


### SQLite 的数据库配置

```ruby
# config/database.yml
default: &default
  adapter: sqlite3
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  timeout: 5000

development:
  <<: *default
  database: storage/development.sqlite3

production:
  <<: *default
  database: storage/production.sqlite3
```


### 单容器部署

```dockerfile
# Dockerfile
FROM ruby:3.3

RUN apt-get update && apt-get install -y \
    libsqlite3-dev \
    libvips \
    ffmpeg

WORKDIR /rails
COPY . .
RUN bundle install
RUN rails assets:precompile

EXPOSE 80 443
CMD ["./bin/rails", "server", "-b", "0.0.0.0"]
```


## 发展理念

### 发货、验证、完善

```ruby
# 1. Merge prototype-quality code to test real usage
# 2. Iterate based on real feedback
# 3. Polish what works, remove what doesn't
```


DHH 尽早合并功能以在生产中进行验证。没有人使用的完美代码比得到反馈的粗糙代码更糟糕。

### 修复根本原因

```ruby
# ✅ Prevent race conditions at the source
config.active_job.enqueue_after_transaction_commit = true

# ❌ Bandaid fix with retries
retry_on ActiveRecord::RecordNotFound, wait: 1.second
```


解决根本问题而不是症状。

### Vanilla Rails 超越抽象

```ruby
# ✅ Direct ActiveRecord
@card.comments.create!(comment_params)

# ❌ Service layer indirection
CreateCommentService.call(@card, comment_params)
```


使用 Rails 约定。只有当真正的痛苦出现时才变得抽象。

## Rails 7.1+ 习语

### params.expect (PR #120)

```ruby
# ✅ Rails 7.1+ style
def card_params
  params.expect(card: [:title, :description, tags: []])
end

# Returns 400 Bad Request if structure invalid

# Old style
def card_params
  params.require(:card).permit(:title, :description, tags: [])
end
```


### StringInquirer (PR #425)

```ruby
# ✅ Readable predicates
event.action.inquiry.completed?
event.action.inquiry.pending?

# Usage
case
when event.action.inquiry.completed?
  send_notification
when event.action.inquiry.failed?
  send_alert
end

# Old style
event.action == "completed"
```


### 正面命名

```ruby
# ✅ Positive names
scope :active, -> { where(active: true) }
scope :visible, -> { where(visible: true) }
scope :published, -> { where.not(published_at: nil) }

# ❌ Negative names
scope :not_deleted, -> { ... }  # Use :active
scope :non_hidden, -> { ... }   # Use :visible
scope :is_not_draft, -> { ... } # Use :published
```


## 提取指南

### 三法则

```ruby
# First time: Just do it inline
def process
  # inline logic
end

# Second time: Still inline, note the duplication
def process_again
  # same logic
end

# Third time: NOW extract
module Processing
  def shared_logic
    # extracted
  end
end
```


等待真正疼痛后再拔牙。

### 从控制器开始，复杂时提取

```ruby
# Phase 1: Logic in controller
def index
  @cards = @board.cards.where(status: params[:status])
end

# Phase 2: Move to model scope
def index
  @cards = @board.cards.by_status(params[:status])
end

# Phase 3: Extract concern if reused
def index
  @cards = @board.cards.filtered(params)
end
```


## 要避免的反模式

### 不要为简单情况添加服务对象

```ruby
# ❌ Over-abstraction
class MessageCreationService
  def initialize(room, params, user)
    @room = room
    @params = params
    @user = user
  end

  def call
    message = @room.messages.build(@params)
    message.creator = @user
    message.save!
    BroadcastService.new(message).call
    message
  end
end

# ✅ Keep it in the model
class Message < ApplicationRecord
  def self.create_with_broadcast!(params)
    create!(params).tap(&:broadcast_create)
  end
end
```


### 不要使用策略对象进行简单身份验证

```ruby
# ❌ Separate policy class
class MessagePolicy
  def initialize(user, message)
    @user = user
    @message = message
  end

  def update?
    @message.creator == @user || @user.admin?
  end
end

# ✅ Method on User model
class User < ApplicationRecord
  def can_administer?(message)
    message.creator == self || admin?
  end
end
```


### 不要嘲笑一切

```ruby
# ❌ Over-mocked test
test "sending message" do
  room = mock("room")
  user = mock("user")
  message = mock("message")

  room.expects(:messages).returns(stub(create!: message))
  message.expects(:broadcast_create)

  MessagesController.new.create
end

# ✅ Test the real thing
test "sending message" do
  sign_in users(:david)
  post room_messages_url(rooms(:watercooler)),
    params: { message: { body: "Hello" } }

  assert_response :success
  assert Message.exists?(body: "Hello")
end
```

