# DHH Ruby 风格资源

链接到源材料、文档和进一步阅读以掌握 DHH 的 Ruby/Rails 风格。

## 主要源代码

### 篝火（一次）
本风格指南源自主要代码库。

- **存储库**：https://github.com/basecamp/once-campfire
- **消息控制器**：https://github.com/basecamp/once-campfire/blob/main/app/controllers/messages_controller.rb
- **JavaScript/刺激**：https://github.com/basecamp/once-campfire/tree/main/app/javascript
- **部署**：带有 SQLite 的单个 Docker 容器

### 其他 37signals 开源
- **实体队列**：https://github.com/rails/solid_queue - 数据库支持的活动作业后端
- **实体缓存**：https://github.com/rails/solid_cache - 数据库支持的 Rails 缓存
- **实心电缆**：https://github.com/rails/solid_cable - 支持数据库的操作电缆适配器
- **Kamal**：https://github.com/basecamp/kamal - 零停机部署工具
- **Turbo**：https://github.com/hotwired/turbo-rails - Hotwire 的类似 SPA 的页面加速器
- **刺激**：https://github.com/hotwired/stimulus - 适度的 JavaScript 框架

## 文章和博客文章

### 控制者组织
- **DHH 如何组织他的 Rails 控制器**：https://jeromedalbert.com/how-dhh-organizes-his-rails-controllers/
  - 关于 REST 纯控制器设计的权威文章
  - 记录了“只有 7 项行动”的理念
  - 展示如何创建新控制器而不是自定义操作

### 测试理念
- **37signals 开发 - 待测试**：https://dev.37signals.com/pending-tests/
  - 37signals 如何处理不完整的测试
  - 测试覆盖率的务实方法
- **37signals 开发 - 关于 QA**：https://dev.37signals.com/all-about-qa/
  - 37signals 的质量保证理念
  - 自动测试和手动测试之间的平衡

### 架构与部署
- **在铁路上部署营火**：https://railway.com/deploy/campfire
  - 单容器部署示例
  - 生产模式中的 SQLite

## 官方文档

### Rails 指南（DHH 的愿景）
- **Rails 原则**：https://rubyonrails.org/doctrine
  - 哲学基础
  - 约定优于配置解释
  - “为程序员的幸福而优化”

### 热线
- **热线**：https://hotwired.dev/
  - 官方 Hotwire 文档
  - 涡轮驱动、框架和流
- **涡轮手册**：https://turbo.hotwired.dev/handbook/introduction
- **刺激手册**：https://stimulus.hotwired.dev/handbook/introduction

### 当前属性
- **Rails API - 当前属性**：https://api.rubyonrails.org/classes/ActiveSupport/CurrentAttributes.html
  - 当前模式的官方文档
  - 线程隔离属性单例

## 视频和讲座

### DHH 主题演讲
- **RailsConf 主题演讲**：在 YouTube 上搜索“DHH RailsConf”
  - Rails 地址的年度状态
  - 哲学和方向讨论

### 热线教程
- **DHH 的 Hotwire 演示**：展示该方法的原始演示
- **GoRails Hotwire 系列**：实用实施教程

## 书籍

### 由 DHH 和 37signals 提供
- **变得真实**：https://basecamp.com/gettingreal
  - 产品开发理念
  - 少即是多的方法
- **远程**：远程工作理念
- **工作不必疯狂**：冷静的公司文化

### Rails 书籍
- **使用 Rails 进行敏捷 Web 开发**：原始的 Rails 书籍
- **The Rails Way**：综合 Rails 模式

## 使用的宝石和工具

### 核心堆栈
```ruby
# Gemfile patterns from Campfire
gem "rails", "~> 8.0"
gem "sqlite3"
gem "propshaft"        # Asset pipeline
gem "importmap-rails"  # JavaScript imports
gem "turbo-rails"      # Hotwire Turbo
gem "stimulus-rails"   # Hotwire Stimulus
gem "solid_queue"      # Job backend
gem "solid_cache"      # Cache backend
gem "solid_cable"      # WebSocket backend
gem "kamal"            # Deployment
gem "thruster"         # HTTP/2 proxy
gem "image_processing" # Active Storage variants
```


### 发展
```ruby
group :development do
  gem "web-console"
  gem "rubocop-rails-omakase"  # 37signals style rules
end

group :test do
  gem "capybara"
  gem "selenium-webdriver"
end
```


## RuboCop 配置

37signals 发布了他们的 RuboCop 规则：
- **rubocop-rails-omakase**：https://github.com/rails/rubocop-rails-omakase
  - 官方 Rails/37signals 风格规则
  - 使用它来执行一致的风格

```yaml
# .rubocop.yml
inherit_gem:
  rubocop-rails-omakase: rubocop.yml

# Project-specific overrides if needed
```


## 社区资源

### 论坛与讨论
- **Ruby on Rails 讨论**：https://discuss.rubyonrails.org/
- **Reddit r/rails**：https://reddit.com/r/rails

### 播客
- **远程 Ruby**：Ruby/Rails 讨论
- **Ruby Rogues**：长期运行的 Ruby 播客
- **The Bike Shed**：Thoughtbot 的开发播客

## 关键哲学文档

### Rails 主义支柱
1.优化程序员的幸福感
2.约定优于配置
3.菜单是omakase
4. 没有一种范式
5. 赞美美丽的代码
6.提供锋利的刀具
7. 价值整合系统
8. 进步重于稳定
9. 搭起一个大帐篷

### DHH 引言要记住

> “绝大多数 Rails 控制器都可以使用相同的七个操作。”

>“如果您要添加自定义操作，则可能缺少控制器。”

> “清晰的代码胜过聪明的代码。”

> “测试文件应该是一封写给代码的情书。”

> “SQLite 对于大多数应用程序来说已经足够了。”

## 版本历史

本风格指南基于：
- 篝火源代码（2024）
- Rails 8.0 约定
- Ruby 3.3 语法首选项
- Hotwire 2.0 模式

最后更新时间：2024 年