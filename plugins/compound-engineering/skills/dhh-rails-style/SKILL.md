---
name: dhh-rails-style
description: 当以 DHH 独特的 37signals 风格编写 Ruby 和 Rails 代码时，应该使用此技能。它适用于编写 Ruby 代码、Rails 应用程序、创建模型、控制器或任何 Ruby 文件。在 Ruby/Rails 代码生成、重构请求、代码审查或用户提及 DHH、37signals、Basecamp、HEY 或 Campfire 风格时触发。体现了 REST 纯度、胖模型、瘦控制器、当前属性、Hotwire 模式以及“清晰胜过聪明”的理念。

---
<objective>
将 37signals/DHH Rails 约定应用于 Ruby 和 Rails 代码。此技能提供从分析生产 37signals 代码库（Fizzy/Campfire）中提取的领域专业知识。
</objective>

<essential_principles>
## 核心理念

“最好的代码是你不写的代码。第二好的代码是明显正确的代码。”

**香草导轨足够了：**
- 服务对象的丰富领域模型
- 自定义操作的 CRUD 控制器
- 对水平代码共享的担忧
- 记录为状态而不是布尔列
- 数据库支持的一切（无 Redis）
- 在获取宝石之前构建解决方案

**他们刻意避免的：**
- devise（自定义〜150行验证）
- pundit/cancancan（模型中的简单角色检查）
- sidekiq（Solid Queue 使用数据库）
- redis（一切数据库）
- view_component（部分工作正常）
- GraphQL（REST 和 Turbo 就足够了）
</essential_principles>

<intake>
你在做什么？

1. **控制器** - REST 映射、关注点、Turbo 响应
2. **模型** - 关注点、状态记录、回调、范围
3. **视图和前端** - Turbo、Stimulus、CSS、部分
4. **架构** - 路由、多租户、身份验证、作业
5. **代码审查** - 根据 DHH 风格审查代码
6. **一般指导** - 理念和惯例

**指定一个数字或描述您的任务。**
</intake>

<routing>
|回应 |参考阅读|
|----------|--------------------|
| 1、“控制器”| [controllers.md](./references/controllers.md) |
| 2、“型号”| [models.md](./references/models.md) |
| 3、“视图”、“前端”、“涡轮”、“刺激”、“css”| [前端.md](./references/frontend.md) |
| 4、“架构”、“路由”、“认证”、“作业”| [架构.md](./references/architecture.md) |
| 5、“回顾”|阅读所有参考资料，然后查看代码 |
| 6、一般任务|根据上下文阅读相关参考资料 |

**阅读相关参考资料后，将模式应用到用户的代码中。**
</routing>

<quick_reference>
## 命名约定

**动词：** `card.close`、`card.gild`、`board.publish`（不是 `set_style` 方法）

**谓词：** `card.closed?`、`card.golden?`（源自相关记录的存在）

**关注点：** 描述能力的形容词（`Closeable`、`Publishable`、`Watchable`）

**控制器：** 名词匹配资源（`Cards::ClosuresController`）

**范围：**
- `chronologically`、`reverse_chronologically`、`alphabetically`、`latest`
- `preloaded`（标准急切加载名称）
- `indexed_by`、`sorted_by`（参数化）

## 休息映射

创建新资源而不是自定义操作：

```
POST /cards/:id/close    → POST /cards/:id/closure
DELETE /cards/:id/close  → DELETE /cards/:id/closure
POST /cards/:id/archive  → POST /cards/:id/archival
```

</quick_reference>

<reference_index>
## 领域知识

`references/` 中的所有详细模式：

|文件 |主题 |
|------|--------|
| [controllers.md](./references/controllers.md) | REST 映射、关注点、Turbo 响应、API 模式 |
| [models.md](./references/models.md) |关注点、状态记录、回调、范围、PORO |
| [前端.md](./references/frontend.md) | Turbo、Stimulus、CSS 架构、视图模式 |
| [架构.md](./references/architecture.md) |路由、身份验证、作业、缓存、多租户、配置 |
| [gems.md](./references/gems.md) |他们使用什么和避免什么，以及为什么 |
</reference_index>

<success_criteria>
在以下情况下，代码遵循 DHH 风格：
- 控制器映射到资源上的 CRUD 动词
- 模型使用对水平行为的关注
- 状态是通过记录而不是布尔值来跟踪的
- 没有不必要的服务对象或抽象
- 数据库支持的解决方案优于外部服务
- 测试使用 Minitest 和固定装置
- Turbo/Stimulus 交互性（无繁重的 JS 框架）
</success_criteria>

<credits>
基于 [Marc Köhlbrugge](https://x.com/marckohlbrugge) 的 [非官方 37signals/DHH Rails 风格指南](https://gist.github.com/marckohlbrugge/d363fb90c89f71bd0c816d24d7642aca)，通过对 Fizzy 代码库的深入分析而生成。
</credits>