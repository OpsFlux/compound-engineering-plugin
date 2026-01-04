# 复合工程插件

由人工智能驱动的开发工具，每次使用都会变得更加智能。让每个工程单元的工作都比上一个更轻松。

## 成分

|组件|计数|
|----------|------|
|代理| 27 | 27
|命令 | 20 |
|技能 | 13 |
| MCP 服务器 | 2 |

## 代理

代理被组织成不同的类别以便于发现。

### 评论 (14)

|代理|描述 |
|--------|-------------|
| `agent-native-reviewer` |验证功能是代理本机的（操作+上下文奇偶校验）|
| `architecture-strategist` |分析架构决策和合规性 |
| `code-simplicity-reviewer` |简单和极简主义的最终通行证 |
| `data-integrity-guardian` |数据库迁移和数据完整性|
| `data-migration-expert` |验证 ID 映射是否与生产匹配，检查交换值 |
| `deployment-verification-agent` |为有风险的数据更改创建Go/No-Go部署清单|
| `dhh-rails-reviewer` | DHH 视角的 Rails 回顾 |
| `kieran-rails-reviewer` |严格约定的 Rails 代码审查 |
| `kieran-python-reviewer` |严格约定的Python代码审查|
| `kieran-typescript-reviewer` |严格约定的 TypeScript 代码审查 |
| `pattern-recognition-specialist` |分析代码中的模式和反模式 |
| `performance-oracle` |性能分析与优化 |
| `security-sentinel` |安全审计和漏洞评估|
| `julik-frontend-races-reviewer` |查看 JavaScript/刺激代码以了解竞争条件 |

### 研究 (4)

|代理|描述 |
|--------|-------------|
| `best-practices-researcher` |收集外部最佳实践和示例 |
| `framework-docs-researcher` |研究框架文档和最佳实践|
| `git-history-analyzer` |分析 git 历史和代码演变 |
| `repo-research-analyst` |研究存储库结构和惯例|

### 设计 (3)

|代理|描述 |
|--------|-------------|
| `design-implementation-reviewer` |验证 UI 实现是否符合 Figma 设计 |
| `design-iterator` |通过系统设计迭代，迭代完善UI |
| `figma-design-sync` |将 Web 实现与 Figma 设计同步 |

### 工作流程 (5)

|代理|描述 |
|--------|-------------|
| `bug-reproduction-validator` |系统地重现和验证错误报告 |
| `every-style-editor` |编辑内容以符合 Every 的风格指南 |
| `lint` |对 Ruby 和 ERB 文件运行 linting 和代码质量检查 |
| `pr-comment-resolver` |解决公关评论并实施修复 |
| `spec-flow-analyzer` |分析用户流量并找出规范中的差距 |

### 文档 (1)

|代理|描述 |
|--------|-------------|
| `ankane-readme-writer` |按照 Ruby gems 的 Ankane 风格模板创建自述文件 |

## 命令

### 工作流程命令

核心工作流程命令使用 `workflows:` 前缀以避免与内置命令发生冲突：

|命令 |描述 |
|---------|-------------|
| `/workflows:plan` |制定实施计划 |
| `/workflows:review` |运行全面的代码审查 |
| `/workflows:work` |系统地执行工作项目 |
| `/workflows:compound` |记录解决的问题以复合团队知识 |

### 实用命令

|命令 |描述 |
|---------|-------------|
| `/deepen-plan` |与每个部分的并行研究代理加强计划|
| `/changelog` |为最近的合并创建引人入胜的变更日志 |
| `/create-agent-skill` |创建或编辑克劳德代码技能 |
| `/generate_command` |生成新的斜杠命令 |
| `/heal-skill` |修复技能文档问题 |
| `/plan_review` |并行多智能体计划审查 |
| `/report-bug` |报告插件中的错误 |
| `/reproduce-bug` |使用日志和控制台重现错误 |
| `/resolve_parallel` |并行解决 TODO 注释 |
| `/resolve_pr_parallel` |并行解决 PR​​ 评论 |
| `/resolve_todo_parallel` |并行解决待办事项 |
| `/triage` |对问题进行分类并确定优先级 |
| `/playwright-test` |在受 PR 影响的页面上运行浏览器测试 |
| `/xcode-test` |在模拟器上构建和测试 iOS 应用程序 |

## 技能

### 建筑与设计

|技能|描述 |
|--------|-------------|
| `agent-native-architecture` |使用提示原生架构构建 AI 代理 |

### 开发工具

|技能|描述 |
|--------|-------------|
| `andrew-kane-gem-writer` |按照 Andrew Kane 的模式编写 Ruby gems |
| `compound-docs` |将已解决的问题捕获为分类文档 |
| `create-agent-skills` |创建 Claude Code 技能的专家指南 |
| `dhh-ruby-style` |以 DHH 的 37signals 风格编写 Ruby/Rails 代码 |
| `dspy-ruby` |使用 DSPy.rb 构建类型安全的 LLM 应用程序 |
| `frontend-design` |创建生产级前端界面 |
| `skill-creator` |创建有效的克劳德代码技能指南 |

### 内容和工作流程

|技能|描述 |
|--------|-------------|
| `every-style-editor` |检查副本以确保 Every 的风格指南合规性 |
| `file-todos` |基于文件的待办事项跟踪系统|
| `git-worktree` |管理 Git 工作树以进行并行开发 |

### 图像生成

|技能|描述 |
|--------|-------------|
| `gemini-imagegen` |使用 Google 的 Gemini API 生成和编辑图像 |

**gemini-imagegen 功能：**
- 文本到图像的生成
- 图像编辑和操作
- 多圈细化
- 多参考图像合成（最多 14 张图像）

**要求：**
- `GEMINI_API_KEY`环境变量
- Python 包：`google-genai`、`pillow`

## MCP 服务器

|服务器|描述 |
|--------|-------------|
| `playwright` |通过 `@playwright/mcp` 实现浏览器自动化 |
| `context7` |通过 Context7 查找框架文档 |

### 剧作家

**提供的工具：**
- `browser_navigate` - 导航至 URL
- `browser_take_screenshot` - 截取屏幕截图
- `browser_click` - 点击元素
- `browser_fill_form` - 填写表格字段
- `browser_snapshot` - 获取无障碍快照
- `browser_evaluate` - 执行 JavaScript

### 上下文7

**提供的工具：**
- `resolve-library-id` - 查找框架/包的库 ID
- `get-library-docs` - 获取特定库的文档

支持 100 多个框架，包括 Rails、React、Next.js、Vue、Django、Laravel 等。

启用插件后，MCP 服务器会自动启动。

## 安装

```bash
claude /plugin install compound-engineering
```


## 已知问题

### MCP 服务器不自动加载

**问题：** 安装插件时，捆绑的 MCP 服务器（Playwright 和 Context7）可能不会自动加载。

**解决方法：** 将它们手动添加到项目的 `.claude/settings.json` 中：

```json
{
  "mcpServers": {
    "playwright": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"],
      "env": {}
    },
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    }
  }
}
```


或者在 `~/.claude/settings.json` 中为所有项目全局添加它们。

## 版本历史

请参阅 [CHANGELOG.md](CHANGELOG.md) 了解详细的版本历史记录。

## 执照

和