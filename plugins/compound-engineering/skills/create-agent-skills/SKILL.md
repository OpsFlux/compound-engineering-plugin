---
name: creating-agent-skills
description: 创建、编写和完善 Claude 代码技能的专家指导。在使用 SKILL.md 文件、创作新技能、改进现有技能或了解技能结构和最佳实践时使用。

---
# 创建代理技能

该技能教授如何按照 Anthropic 的官方规范创建有效的 Claude 代码技能。

## 核心原则

### 1. 技能是提示

所有提示最佳实践均适用。说得清楚、直接。假设克劳德很聪明——只添加克劳德没有的上下文。

### 2. 标准 Markdown 格式

使用 YAML frontmatter + markdown body。 **无 XML 标签** - 使用标准 Markdown 标题。

```markdown
---
name: my-skill-name
description: What it does and when to use it
---

# My Skill Name

## Quick Start
Immediate actionable guidance...

## Instructions
Step-by-step procedures...

## Examples
Concrete usage examples...
```


### 3. 渐进式披露

将 SKILL.md 保持在 500 行以内。将详细内容拆分为参考文件。仅加载需要的内容。

```
my-skill/
├── SKILL.md              # Entry point (required)
├── reference.md          # Detailed docs (loaded when needed)
├── examples.md           # Usage examples
└── scripts/              # Utility scripts (executed, not loaded)
```


### 4. 有效的描述

描述字段支持技能发现。包括该技能的作用以及何时使用它。用第三人称写。

**好：**
```yaml
description: Extracts text and tables from PDF files, fills forms, merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```


**不好：**
```yaml
description: Helps with documents
```


## 技能结构

### 必需的 Frontmatter

|领域|必填|最大长度|描述 |
|--------|----------|------------|------------|
| `name` |是的 | 64 个字符 |仅小写字母、数字、连字符 |
| `description` |是的 | 1024 个字符 |它的作用以及何时使用它 |
| `allowed-tools` |没有 | - |克劳德无需询问即可使用的工具 |
| `model` |没有 | - |具体使用型号|

### 命名约定

使用**动名词形式**（动词 + -ing）作为技能名称：

- `processing-pdfs`
- `analyzing-spreadsheets`
- `generating-commit-messages`
- `reviewing-code`

避免：`helper`、`utils`、`tools`、`anthropic-*`、`claude-*`

### 身体结构

使用标准 Markdown 标题：

```markdown
# Skill Name

## Quick Start
Fastest path to value...

## Instructions
Core guidance Claude follows...

## Examples
Input/output pairs showing expected behavior...

## Advanced Features
Additional capabilities (link to reference files)...

## Guidelines
Rules and constraints...
```


## 你想做什么？

1. **创建新技能** - 从头开始构建
2. **审核现有技能** - 检查最佳实践
3. **添加组件** - 添加工作流程/参考/示例
4. **获得指导** - 了解技能设计

## 创造新技能

### 第 1 步：选择类型

**简单技能（单档）：**
- 500行以下
- 独立的指导
- 没有复杂的工作流程

**渐进披露技巧（多个文件）：**
- SKILL.md 作为概述
- 详细文档的参考文件
- 实用程序脚本

### 步骤 2：创建 SKILL.md

```markdown
---
name: your-skill-name
description: [What it does]. Use when [trigger conditions].
---

# Your Skill Name

## Quick Start

[Immediate actionable example]

```[language]

[代码示例]
```

## Instructions

[Core guidance]

## Examples

**Example 1:**
Input: [description]
Output:
```

[结果]
```

## Guidelines

- [Constraint 1]
- [Constraint 2]
```


### 步骤 3：添加参考文件（如果需要）

SKILL.md 详细内容链接：

```markdown
For API reference, see [REFERENCE.md](REFERENCE.md).
For form filling guide, see [FORMS.md](FORMS.md).
```


保留 SKILL.md 的参考**一层深度**。

### 步骤 4：添加脚本（如果需要）

脚本执行时无需加载到上下文中：

```markdown
## Utility Scripts

Extract fields:
```bash

python scripts/analyze.py input.pdf > fields.json
```
```


### 步骤 5：实际使用情况测试

1. 使用实际任务进行测试，而不是测试场景
2.观察克劳德挣扎的地方
3. 基于真实行为进行细化
4. 使用 Haiku、Sonnet 和 Opus 进行测试

## 审核现有技能

检查此标题：

- [ ] 有效的 YAML frontmatter（名称 + 描述）
- [ ] 描述包含触发关键字
- [ ] 使用标准 Markdown 标题（不是 XML 标签）
- [ ] SKILL.md 500行以下
- [ ] 引用一层深
- [ ] 例子是具体的，而不是抽象的
- [ ] 一致的术语
- [ ] 没有时间敏感的信息
- [ ] 脚本明确处理错误

## 常见模式

### 模板模式

提供输出模板以获得一致的结果：

```markdown
## Report Template

```markdown

# [分析标题]

## 执行摘要
【一段概述】

## 主要发现
- 发现1
- 发现2

## 建议
1. [行动项目]
2. [行动项目]
```
```


### 工作流程模式

对于复杂的多步骤任务：

```markdown
## Migration Workflow

Copy this checklist:

```

- [ ] 步骤一：备份数据库
- [ ] 步骤 2：运行迁移脚本
- [ ] 步骤 3：验证输出
- [ ] 步骤 4：更新配置
```

**Step 1: Backup database**
Run: `./scripts/backup.sh`
...
```


### 条件模式

决策点指南：

```markdown
## Choose Your Approach

**Creating new content?** Follow "Creation workflow" below.
**Editing existing?** Follow "Editing workflow" below.
```


## 要避免的反模式

- **正文中的 XML 标签** - 使用 Markdown 标题代替
- **模糊的描述** - 使用具体的触发关键字
- **深度嵌套** - 将 SKILL.md 中的引用保留一层
- **选项太多** - 提供默认的逃生舱口
- **Windows 路径** - 始终使用正斜杠
- **踢克劳德** - 脚本应该处理错误
- **时间敏感信息** - 使用“旧模式”部分代替

## 参考文件

有关详细指导，请参阅：

- [official-spec.md](references/official-spec.md) - Anthropic的官方技能规范
- [best-practices.md](references/best-practices.md) - 技能创作最佳实践

## 成功标准

结构良好的技能：
- 具有有效的 YAML frontmatter 以及描述性名称和描述
- 使用标准 Markdown 标题（不是 XML 标签）
- 将 SKILL.md 保持在 500 行以内
- 详细内容的参考文件链接
- 包括带有输入/输出对的具体示例
- 已经过实际使用测试

资料来源：
- [代理技能 - 克劳德代码文档](https://code.claude.com/docs/en/skills)
- [技能创作最佳实践](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [GitHub - anthropics/技能](https://github.com/anthropics/skills)