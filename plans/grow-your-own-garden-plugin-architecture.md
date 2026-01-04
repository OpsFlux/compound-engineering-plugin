# 种植你自己的花园：自适应代理生态系统

> **问题：** https://github.com/EveryInc/compound-engineering-plugin/issues/20

## 想法

每个人都种植自己的花园，但我们都使用相同的过程。

从**种子**开始（最小核心：`/plan`、`/work`、`/review`、`/compound`）。每个 `/compound` 循环都可以根据您正在处理的内容建议添加代理，例如构建测试套件以防止回归，但需要代码审查专业知识。

## 当前问题

- 整体插件：24 个代理，用户使用 ~30%
- 无个性化（Rails 开发和 Python 开发的代理相同）
- 不适应的静态集合

## 建议的解决方案

### 种子（核心插件）

4 个命令 + 最少的代理：

|组件|包含什么 |
|------------|------------------|
|命令 | `/plan`、`/work`、`/review`、`/compound` |
|审查代理|安全性、性能、简单性、架构、模式|
|研究代理|最佳实践、框架文档、git 历史记录、repo 分析师 |
|技能 |复合文档、文件待办事项、git-worktree |
| MCP 服务器 |剧作家，背景7 |

### 增长循环

每 `/compound` 之后：

```
✅ Learning documented

💡 It looks like you're using Rails.
   Would you like to add the "DHH Rails Reviewer"?

   [y] Yes  [n] No  [x] Never ask
```


新代理的三个来源：
1. **预定义** - “您正在使用 Rails，添加 DHH 审阅者吗？”
2. **动态** - “您正在使用演员模型，创建专家？”
3. **自定义** - “想要为此模式创建代理吗？”

### 代理存储

```
.claude/agents/       → Project-specific (highest priority)
~/.claude/agents/     → User's garden
plugin/agents/        → From installed plugins
```


## 实施阶段

### 第 1 阶段：拆分插件
- 使用特定于框架的代理（Rails、Python、TypeScript、前端）创建`agent-library/`
- 将`compound-engineering`作为通用代理的核心
- 没有重大变化——现有用户不受影响

### 第 2 阶段：代理发现
- `/review` 发现来自所有三个地点的代理
- 项目代理覆盖用户代理覆盖插件代理

### 第 3 阶段：通过 /compound 实现增长
- 检测技术堆栈（Gemfile、package.json 等）
- 记录学习内容后建议相关代理
- 将接受的代理安装到`~/.claude/agents/`

### 第四阶段：管理
- `/agents list` - 看看你的花园
- `/agents add <name>` - 从库添加
- `/agents disable <name>` - 暂时禁用

## 什么去哪里

**核心（种子）：** 11 个与框架无关的代理
- 安全哨兵、性能预言机、代码简单性审查者
- 架构策略师、模式识别专家
- 4 个研究代理，2 个工作流程代理

**代理库：** 10个专业代理
- Rails：kieran-rails、dhh-rails、数据完整性 (3)
- Python：kieran-python (1)
- TypeScript：kieran-typescript (1)
- 前端：julik-races、设计迭代器、设计评审器、figma-sync (4)
- 社论：每种风格的编辑器 (1)

## 关键约束

Claude Code 不支持插件依赖项。每个插件必须是独立的。用户手动安装他们需要的内容，或者我们建议通过`/compound`添加。

## 验收标准

- [ ] 核心插件与通用代理独立工作
- [ ] `/compound` 根据检测到的技术堆栈建议代理
- [ ] 用户可以接受/拒绝建议
- [ ] `/agents`花园管理命令
- [ ] 对现有用户没有重大更改