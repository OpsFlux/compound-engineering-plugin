---
name: release-docs
description: 使用当前插件组件构建和更新文档站点

argument-hint: "[optional: --dry-run to preview changes without writing]"
---
# 发布文档命令

您是复合工程插件的文档生成器。您的工作是确保 `plugins/compound-engineering/docs/` 的文档站点始终与实际插件组件保持同步。

## 概述

该文档站点是一个基于 Evil Martians LaunchKit 模板的静态 HTML/CSS/JS 站点。每当出现以下情况时就需要重新生成：

- 添加、删除或修改代理
- 添加、删除或修改命令
- 技能被添加、删除或修改
- 添加、删除或修改 MCP 服务器

## 第 1 步：清点当前组件

首先，统计并列出所有当前组件：

```bash
# Count agents
ls plugins/compound-engineering/agents/*.md | wc -l

# Count commands
ls plugins/compound-engineering/commands/*.md | wc -l

# Count skills
ls -d plugins/compound-engineering/skills/*/ 2>/dev/null | wc -l

# Count MCP servers
ls -d plugins/compound-engineering/mcp-servers/*/ 2>/dev/null | wc -l
```


读取所有组件文件以获取其元数据：

### 代理
对于 `plugins/compound-engineering/agents/*.md` 中的每个代理文件：
- 提取前面的内容（名称、描述）
- 记下类别（评论、研究、工作流程、设计、文档）
- 从内容中获取关键职责

### 命令
对于 `plugins/compound-engineering/commands/*.md` 中的每个命令文件：
- 提取前面的内容（名称、描述、参数提示）
- 分类为工作流程或实用命令

### 技能
对于`plugins/compound-engineering/skills/*/`中的每个技能目录：
- 阅读 SKILL.md 文件中的 frontmatter（名称、描述）
- 记下任何脚本或支持文件

### MCP 服务器
对于 `plugins/compound-engineering/mcp-servers/*/` 中的每个 MCP 服务器：
- 阅读配置和README
- 列出提供的工具

## 步骤 2：更新文档页面

### 2a。更新`docs/index.html`

用准确的计数更新统计部分：
```html
<div class="stats-grid">
  <div class="stat-card">
    <span class="stat-number">[AGENT_COUNT]</span>
    <span class="stat-label">Specialized Agents</span>
  </div>
  <!-- Update all stat cards -->
</div>
```


确保组件摘要部分准确列出关键组件。

### 2b。更新`docs/pages/agents.html`

重新生成完整的代理参考页面：
- 按类别对代理进行分组（审查、研究、工作流程、设计、文档）
- 包括每个代理：
  - 名称和描述
  - 主要职责（项目符号清单）
  - 使用示例：`claude agent [agent-name] "your message"`
  - 使用案例

### 2c。更新`docs/pages/commands.html`

重新生成完整的命令参考页面：
- 按类型（工作流程、实用程序）对命令进行分组
- 每个命令包括：
  - 名称和描述
  - 参数（如果有）
  - 流程/工作流程步骤
  - 用法示例

### 2d。更新`docs/pages/skills.html`

重新生成完整技能参考页面：
- 按类别对技能进行分组（开发工具、内容和工作流程、图像生成）
- 每项技能包括：
  - 名称和描述
  - 用法：`claude skill [skill-name]`
  - 特性和功能

### 2e。更新`docs/pages/mcp-servers.html`

重新生成 MCP 服务器参考页面：
- 对于每个服务器：
  - 名称和目的
  - 提供工具
  - 配置详细信息
  - 支持的框架/服务

## 步骤 3：更新元数据文件

确保计数在以下方面保持一致：

1. **`plugins/compound-engineering/.claude-plugin/plugin.json`**
   - 使用正确的计数更新`description`
   - 使用计数更新`components`对象
   - 使用当前项目更新`agents`、`commands`数组

2. **`.claude-plugin/marketplace.json`**
   - 使用正确的计数更新插件`description`

3. **`plugins/compound-engineering/README.md`**
   - 用计数更新介绍段落
   - 更新组件列表

## 步骤 4：验证

运行验证检查：

```bash
# Validate JSON files
cat .claude-plugin/marketplace.json | jq .
cat plugins/compound-engineering/.claude-plugin/plugin.json | jq .

# Verify counts match
echo "Agents in files: $(ls plugins/compound-engineering/agents/*.md | wc -l)"
grep -o "[0-9]* specialized agents" plugins/compound-engineering/docs/index.html

echo "Commands in files: $(ls plugins/compound-engineering/commands/*.md | wc -l)"
grep -o "[0-9]* slash commands" plugins/compound-engineering/docs/index.html
```


## 步骤 5：报告更改

提供更新内容的摘要：

```
## Documentation Release Summary

### Component Counts
- Agents: X (previously Y)
- Commands: X (previously Y)
- Skills: X (previously Y)
- MCP Servers: X (previously Y)

### Files Updated
- docs/index.html - Updated stats and component summaries
- docs/pages/agents.html - Regenerated with X agents
- docs/pages/commands.html - Regenerated with X commands
- docs/pages/skills.html - Regenerated with X skills
- docs/pages/mcp-servers.html - Regenerated with X servers
- plugin.json - Updated counts and component lists
- marketplace.json - Updated description
- README.md - Updated component lists

### New Components Added
- [List any new agents/commands/skills]

### Components Removed
- [List any removed agents/commands/skills]
```


## 试运行模式

如果指定了`--dry-run`：
- 执行所有库存和验证步骤
- 报告将要更新的内容
- 不要写入任何文件
- 显示建议更改的差异预览

## 错误处理

- 如果组件文件有无效的frontmatter，报告错误并跳过
- 如果 JSON 验证失败，则报告并中止
- 始终保持有效状态 - 不要部分更新

## 发布后

发布成功后：
1.建议更新CHANGELOG.md以进行文档更改
2. 提醒提交消息：`docs: Update documentation site to match plugin components`
3.提醒推送变更

## 用法示例

```bash
# Full documentation release
claude /release-docs

# Preview changes without writing
claude /release-docs --dry-run

# After adding new agents
claude /release-docs
```

