---
name: dhh-ruby-style
description: 当以 DHH 独特的 37signals 风格编写 Ruby 和 Rails 代码时，应该使用此技能。它适用于编写 Ruby 代码、Rails 应用程序、创建模型、控制器或任何 Ruby 文件。在 Ruby/Rails 代码生成、重构请求、代码审查或用户提及 DHH、37signals、Basecamp、HEY 或 Campfire 风格时触发。体现了 REST 纯度、胖模型、瘦控制器、当前属性、Hotwire 模式以及“清晰胜过聪明”的理念。

---
# DHH Ruby/Rails 风格指南

按照 DHH 的理念编写 Ruby 和 Rails 代码：**清晰胜于聪明**，**约定胜于配置**，**开发人员的幸福感**高于一切。

## 快速参考

### 控制器操作
- **仅 7 个 REST 操作**：`index`、`show`、`new`、`create`、`edit`、`update`、`destroy`
- **新行为？** 创建新控制器，而不是自定义操作
- **动作长度**：最多 1-5 行
- **空操作很好**：让 Rails 约定处理渲染

```ruby
class MessagesController < ApplicationController
  before_action :set_message, only: %i[ show edit update destroy ]

  def index
    @messages = @room.messages.with_creator.last_page
    fresh_when @messages
  end

  def show
  end

  def create
    @message = @room.messages.create_with_attachment!(message_params)
    @message.broadcast_create
  end

  private
    def set_message
      @message = @room.messages.find(params[:id])
    end

    def message_params
      params.require(:message).permit(:body, :attachment)
    end
end
```


### 私有方法缩进
在 `private` 关键字下将私有方法缩进一级：

```ruby
  private
    def set_message
      @message = Message.find(params[:id])
    end

    def message_params
      params.require(:message).permit(:body)
    end
```


### 模型设计（胖模型）
模型拥有自己的业务逻辑、授权和广播：

```ruby
class Message < ApplicationRecord
  belongs_to :room
  belongs_to :creator, class_name: "User"
  has_many :mentions

  scope :with_creator, -> { includes(:creator) }
  scope :page_before, ->(cursor) { where("id < ?", cursor.id).order(id: :desc).limit(50) }

  def broadcast_create
    broadcast_append_to room, :messages, target: "messages"
  end

  def mentionees
    mentions.includes(:user).map(&:user)
  end
end

class User < ApplicationRecord
  def can_administer?(message)
    message.creator == self || admin?
  end
end
```


### 当前属性
使用 `Current` 作为请求上下文，切勿在任何地方传递 `current_user`：

```ruby
class Current < ActiveSupport::CurrentAttributes
  attribute :user, :session
end

# Usage anywhere in app
Current.user.can_administer?(@message)
```


### Ruby 语法首选项

```ruby
# Symbol arrays with spaces inside brackets
before_action :set_message, only: %i[ show edit update destroy ]

# Modern hash syntax exclusively
params.require(:message).permit(:body, :attachment)

# Single-line blocks with braces
users.each { |user| user.notify }

# Ternaries for simple conditionals
@room.direct? ? @room.users : @message.mentionees

# Bang methods for fail-fast
@message = Message.create!(params)
@message.update!(message_params)

# Predicate methods with question marks
@room.direct?
user.can_administer?(@message)
@messages.any?

# Expression-less case for cleaner conditionals
case
when params[:before].present?
  @room.messages.page_before(params[:before])
when params[:after].present?
  @room.messages.page_after(params[:after])
else
  @room.messages.last_page
end
```


### 命名约定

|元素|大会 |示例|
|--------|------------|---------|
| Setter 方法 | `set_` 前缀 | `set_message`、`set_room` |
|参数方法| `{model}_params` | `message_params` |
|协会名称|语义的，非通用的 | `creator` 不是`user` |
|范围 |可链接、描述性 | `with_creator`、`page_before` |
|谓词 |以 `?` 结尾 | `direct?`、`can_administer?` |

### Hotwire/Turbo 模式
广播是模型责任：

```ruby
# In model
def broadcast_create
  broadcast_append_to room, :messages, target: "messages"
end

# In controller
@message.broadcast_replace_to @room, :messages,
  target: [ @message, :presentation ],
  partial: "messages/presentation",
  attributes: { maintain_scroll: true }
```


### 错误处理
拯救特定的异常，使用 bang 方法快速失败：

```ruby
def create
  @message = @room.messages.create_with_attachment!(message_params)
  @message.broadcast_create
rescue ActiveRecord::RecordNotFound
  render action: :room_not_found
end
```


### 架构首选项

|传统| DHH 方式 |
|-------------|---------|
| PostgreSQL | SQLite（用于单租户）|
| Redis + Sidekiq |坚实的队列|
| Redis缓存|固态缓存|
|库伯内特 |单个 Docker 容器 |
|服务对象|胖模特|
|政策对象（专家）|用户模型授权|
|工厂机器人 |固定装置|

## 详细参考资料

有关全面的模式和示例，请参阅：
- [patterns.md](./references/patterns.md) - 完整的代码模式及解释
- [resources.md](./references/resources.md) - 源材料和进一步阅读的链接

## 理念总结

1. **REST纯度**：仅7个动作；新的控制器变化
2. **胖模型**：模型中的授权、广播、业务逻辑
3. **瘦控制器**：1-5行动作；提取复杂性
4. **约定优于配置**：空方法，隐式渲染
5. **最小抽象**：简单情况下没有服务对象
6. **当前属性**：无处不在的线程本地请求上下文
7. **Hotwire-first**：模型级广播、Turbo Streams、刺激
8. **可读代码**：语义命名，小方法，无需注释
9. **实用测试**：系统测试优于单元测试，真正的集成