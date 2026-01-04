# 每个市场 - Claude 代码插件市场

该存储库是一个 Claude Code 插件市场，它将 `compound-engineering` 插件分发给使用 AI 支持的工具进行构建的开发人员。

## 存储库结构

```
every-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog (lists available plugins)
├── docs/                         # Documentation site (GitHub Pages)
│   ├── index.html                # Landing page
│   ├── css/                      # Stylesheets
│   ├── js/                       # JavaScript
│   └── pages/                    # Reference pages
└── plugins/
    └── compound-engineering/   # The actual plugin
        ├── .claude-plugin/
        │   └── plugin.json        # Plugin metadata
        ├── agents/                # 24 specialized AI agents
        ├── commands/              # 13 slash commands
        ├── skills/                # 11 skills
        ├── mcp-servers/           # 2 MCP servers (playwright, context7)
        ├── README.md              # Plugin documentation
        └── CHANGELOG.md           # Version history
```


## 理念：复合工程

**每个工程工作单元都应该使后续工作单元变得更容易，而不是更困难。**

使用此存储库时，请遵循复合工程流程：

1. **计划** → 了解所需的改变及其影响
2. **委托** → 使用AI工具帮助实施
3. **评估** → 验证更改是否按预期工作
4. **编纂** → 使用学习内容更新此 CLAUDE.md

## 使用此存储库

### 添加新插件

1.创建插件目录：`plugins/new-plugin-name/`
2.添加插件结构：
   ```
   plugins/new-plugin-name/
   ├── .claude-plugin/plugin.json
   ├── agents/
   ├── commands/
   └── README.md
   ```

3. 更新`.claude-plugin/marketplace.json`以包含新插件
4.提交前在本地测试

### 更新复合工程插件

添加/删除代理、命令或技能时，请遵循以下清单：

#### 1. 准确计数所有成分

```bash
# Count agents
ls plugins/compound-engineering/agents/*.md | wc -l

# Count commands
ls plugins/compound-engineering/commands/*.md | wc -l

# Count skills
ls -d plugins/compound-engineering/skills/*/ 2>/dev/null | wc -l
```


#### 2. 使用正确的计数更新所有描述字符串

该描述出现在多个位置并且必须在所有位置都匹配：

- [ ] `plugins/compound-engineering/.claude-plugin/plugin.json` → `description` 字段
- [ ] `.claude-plugin/marketplace.json` → 插件`description`字段
- [ ] `plugins/compound-engineering/README.md` → 介绍段落

格式：`"Includes X specialized agents, Y commands, and Z skill(s)."`

#### 3.更新版本号

添加新功能时，将版本升级为：

- [ ] `plugins/compound-engineering/.claude-plugin/plugin.json` → `version`
- [ ] `.claude-plugin/marketplace.json` → 插件`version`

#### 4.更新文档

- [ ] `plugins/compound-engineering/README.md` → 列出所有组件
- [ ] `plugins/compound-engineering/CHANGELOG.md` → 文件变更
- [ ] `CLAUDE.md` → 如果需要更新结构图

#### 5. 重建文档站点

运行release-docs命令来更新所有文档页面：

```bash
claude /release-docs
```


这将：
- 更新登陆页面上的统计信息
- 重新生成参考页面（代理、命令、技能、MCP 服务器）
- 更新变更日志页面
- 验证所有计数与实际文件匹配

#### 6. 验证 JSON 文件

```bash
cat .claude-plugin/marketplace.json | jq .
cat plugins/compound-engineering/.claude-plugin/plugin.json | jq .
```


#### 6. 提交前验证

```bash
# Ensure counts in descriptions match actual files
grep -o "Includes [0-9]* specialized agents" plugins/compound-engineering/.claude-plugin/plugin.json
ls plugins/compound-engineering/agents/*.md | wc -l
```


### Marketplace.json 结构

Marketplace.json 遵循官方 Claude Code 规范：

```json
{
  "name": "marketplace-identifier",
  "owner": {
    "name": "Owner Name",
    "url": "https://github.com/owner"
  },
  "metadata": {
    "description": "Marketplace description",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "description": "Plugin description",
      "version": "1.0.0",
      "author": { ... },
      "homepage": "https://...",
      "tags": ["tag1", "tag2"],
      "source": "./plugins/plugin-name"
    }
  ]
}
```


**仅包含官方规范中的字段。**不要添加自定义字段，例如：

- `downloads`、`stars`、`rating`（仅显示）
- `categories`、`featured_plugins`、`trending`（不在规格中）
- `type`、`verified`、`featured`（不在规格中）

### Plugin.json 结构

每个插件都有自己的plugin.json，其中包含详细的元数据：

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": { ... },
  "keywords": ["keyword1", "keyword2"],
  "components": {
    "agents": 15,
    "commands": 6,
    "hooks": 2
  },
  "agents": {
    "category": [
      {
        "name": "agent-name",
        "description": "Agent description",
        "use_cases": ["use-case-1", "use-case-2"]
      }
    ]
  },
  "commands": {
    "category": ["command1", "command2"]
  }
}
```


## 文档站点

文档站点位于存储库根目录中的`/docs`（对于 GitHub Pages）。该网站使用纯 HTML/CSS/JS 构建（基于 Evil Martians 的 LaunchKit 模板），无需构建步骤即可查看。

### 文档结构

```
docs/
├── index.html           # Landing page with stats and philosophy
├── css/
│   ├── style.css        # Main styles (LaunchKit-based)
│   └── docs.css         # Documentation-specific styles
├── js/
│   └── main.js          # Interactivity (theme toggle, mobile nav)
└── pages/
    ├── getting-started.html  # Installation and quick start
    ├── agents.html           # All 24 agents reference
    ├── commands.html         # All 13 commands reference
    ├── skills.html           # All 11 skills reference
    ├── mcp-servers.html      # MCP servers reference
    └── changelog.html        # Version history
```


### 保持文档最新

**重要：** 对代理、命令、技能或 MCP 服务器进行任何更改后，运行：

```bash
claude /release-docs
```


这个命令：
1. 统计当前所有组件
2.读取所有代理/命令/技能/MCP文件
3. 重新生成所有参考页面
4. 更新登陆页面上的统计信息
5. 从 CHANGELOG.md 更新变更日志
6. 验证所有文件的计数匹配

### 手动更新

如果您需要手动更新文档：

1. **着陆页统计数据** - 更新 `docs/index.html` 中的数字：
   ```html
   <span class="stat-number">24</span>  <!-- agents -->
   <span class="stat-number">13</span>  <!-- commands -->
   ```


2. **参考页** - `docs/pages/` 中的每一页记录了该类别中的所有组件

3. **变更日志** - `docs/pages/changelog.html` 以 HTML 格式镜像 `CHANGELOG.md`

### 本地查看文档

由于文档是静态 HTML，您可以直接查看它们：

```bash
# Open in browser
open docs/index.html

# Or start a local server
cd docs
python -m http.server 8000
# Then visit http://localhost:8000
```


## 测试变更

### 本地测试

1.本地安装市场：

   ```bash
   claude /plugin marketplace add /Users/yourusername/every-marketplace
   ```


2.安装插件：

   ```bash
   claude /plugin install compound-engineering
   ```


3. 测试代理和命令：
   ```bash
   claude /review
   claude agent kieran-rails-reviewer "test message"
   ```


### 验证 JSON

提交之前，请确保 JSON 文件有效：

```bash
cat .claude-plugin/marketplace.json | jq .
cat plugins/compound-engineering/.claude-plugin/plugin.json | jq .
```


## 常见任务

### 添加新代理

1. 创建`plugins/compound-engineering/agents/new-agent.md`
2. 更新plugin.json代理数量和代理列表
3.更新README.md代理列表
4. 用`claude agent new-agent "test"`测试

### 添加新命令

1. 创建`plugins/compound-engineering/commands/new-command.md`
2.更新plugin.json命令计数和命令列表
3.更新README.md命令列表
4. 用`claude /new-command`测试

### 添加新技能

1.创建技能目录：`plugins/compound-engineering/skills/skill-name/`
2.添加技能结构：
   ```
   skills/skill-name/
   ├── SKILL.md           # Skill definition with frontmatter (name, description)
   └── scripts/           # Supporting scripts (optional)
   ```

3.使用新的技能计数更新plugin.json描述
4. 使用新的技能计数更新 Marketplace.json 描述
5.使用技能文档更新README.md
6. 更新 CHANGELOG.md 添加
7. 用`claude skill skill-name`测试

**技能文件格式（SKILL.md）：**
```markdown
---
name: skill-name
description: Brief description of what the skill does
---

# Skill Title

Detailed documentation...
```


### 更新标签/关键字

标签应反映复合工程理念：

- 使用：`ai-powered`、`compound-engineering`、`workflow-automation`、`knowledge-management`
- 避免：特定于框架的标签，除非插件是特定于框架的

## 提交约定

提交消息请遵循以下模式：

- `Add [agent/command name]` - 添加新功能
- `Remove [agent/command name]` - 删除功能
- `Update [file] to [what changed]` - 更新现有文件
- `Fix [issue]` - 错误修复
- `Simplify [component] to [improvement]` - 重构

包括克劳德代码页脚：

```
🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```


## 需要更多信息时搜索的资源

- [Claude 代码插件文档](https://docs.claude.com/en/docs/claude-code/plugins)
- [插件市场文档](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [插件参考](https://docs.claude.com/en/docs/claude-code/plugins-reference)

## 主要经验教训

_本节记录了我们在这个存储库上工作时学到的重要知识。_

### 2024-11-22：添加了 gemini-imagegen 技能并修复了组件数量

在插件中添加了第一个技能，发现组件数量错误（说是15个代理，实际上有17个）。创建了一个用于更新插件的全面清单，以防止将来出现这种情况。

**学习：** 在更新描述之前始终对实际文件进行计数。计数出现在多个位置（plugin.json、marketplace.json、README.md）并且必须全部匹配。使用上面清单中的验证命令。

### 2024-10-09：简化marketplace.json以匹配官方规范

最初的 Marketplace.json 包含许多自定义字段（下载、星级、评级、类别、趋势），这些字段不属于 Claude Code 规范的一部分。我们简化为仅包括：

- 必需：`name`、`owner`、`plugins`
- 可选：`metadata`（带有说明和版本）
- 插件条目：`name`、`description`、`version`、`author`、`homepage`、`tags`、`source`

**学习：** 坚持官方规范。自定义字段可能会让用户感到困惑或破坏与未来版本的兼容性。