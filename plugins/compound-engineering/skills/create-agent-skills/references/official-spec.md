# Anthropic官方技能说明

来源：[code.claude.com/docs/en/skills](https://code.claude.com/docs/en/skills)

## SKILL.md 文件结构

每项技能都需要一个 `SKILL.md` 文件，其中包含 YAML frontmatter 和 Markdown 指令。

### 基本格式

```markdown
---
name: your-skill-name
description: Brief description of what this Skill does and when to use it
---

# Your Skill Name

## Instructions
Provide clear, step-by-step guidance for Claude.

## Examples
Show concrete examples of using this Skill.
```


## 必需的 Frontmatter 字段

|领域|必填|描述 |
|--------|----------|-------------|
| `name` |是的 |技能名称仅使用小写字母、数字和连字符（最多 64 个字符）。应与目录名称匹配。 |
| `description` |是的 |该技能的用途以及何时使用它（最多 1024 个字符）。克劳德用它来决定何时应用技能。 |
| `allowed-tools` |没有 |当该技能激活时，克劳德无需征得许可即可使用工具。示例：`Read, Grep, Glob` |
| `model` |没有 |该技能激活时使用的特定模型（例如，`claude-sonnet-4-20250514`）。默认为对话模型。 |

## 技能位置和优先级

```
Enterprise (highest priority) → Personal → Project → Plugin (lowest priority)
```


|类型 |路径|适用于 |
|------|------|------------|
| **企业** |查看托管设置 |组织中的所有用户 |
| **个人** | `~/.claude/skills/` |您，跨所有项目|
| **项目** | `.claude/skills/` |任何在存储库中工作的人 |
| **插件** |捆绑插件 |任何安装了插件的人 |

## 技能如何发挥作用

1. **发现**：克劳德在启动时仅加载名称和描述
2. **激活**：当您的请求与技能描述相符时，克劳德会要求确认
3. **执行**：克劳德按照技能的指示，加载引用的文件

**关键原则**：技能是**模型调用的** - 克劳德根据您的请求自动决定使用哪些技能。

## 渐进式披露模式

通过链接到支持文件，将 `SKILL.md` 控制在 500 行以下：

```
my-skill/
├── SKILL.md (required - overview and navigation)
├── reference.md (detailed API docs - loaded when needed)
├── examples.md (usage examples - loaded when needed)
└── scripts/
    └── helper.py (utility script - executed, not loaded)
```


### 带有参考文献的 SKILL.md 示例

```markdown
---
name: pdf-processing
description: Extract text, fill forms, merge PDFs. Use when working with PDF files, forms, or document extraction. Requires pypdf and pdfplumber packages.
allowed-tools: Read, Bash(python:*)
---

# PDF Processing

## Quick start

Extract text:
```python

导入 pdfplumber
使用 pdfplumber.open("doc.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```

For form filling, see [FORMS.md](FORMS.md).
For detailed API reference, see [REFERENCE.md](REFERENCE.md).

## Requirements

Packages must be installed:
```bash

pip 安装 pypdf pdfplumber
```
```


## 限制工具访问

```yaml
---
name: reading-files-safely
description: Read files without making changes. Use when you need read-only file access.
allowed-tools: Read, Grep, Glob
---
```


好处：
- 不应修改文件的只读技能
- 特定任务的范围有限
- 安全敏感的工作流程

## 撰写有效的描述

`description` 字段支持技能发现，并且应包括技能的用途和使用时间。

**始终以第三人称书写。** 描述被注入到系统提示符中。

- **好：**“处理 Excel 文件并生成报告”
- **避免：**“我可以帮你处理Excel文件”
- **避免：**“您可以使用它来处理 Excel 文件”

**具体并包括关键术语：**

```yaml
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```


**避免模糊的描述：**

```yaml
description: Helps with documents  # Too vague!
```


## 完整示例：提交消息生成器

```markdown
---
name: generating-commit-messages
description: Generates clear commit messages from git diffs. Use when writing commit messages or reviewing staged changes.
---

# Generating Commit Messages

## Instructions

1. Run `git diff --staged` to see changes
2. I'll suggest a commit message with:
   - Summary under 50 characters
   - Detailed description
   - Affected components

## Best practices

- Use present tense
- Explain what and why, not how
```


## 完整示例：代码解释技巧

```markdown
---
name: explaining-code
description: Explains code with visual diagrams and analogies. Use when explaining how code works, teaching about a codebase, or when the user asks "how does this work?"
---

# Explaining Code

When explaining code, always include:

1. **Start with an analogy**: Compare the code to something from everyday life
2. **Draw a diagram**: Use ASCII art to show the flow, structure, or relationships
3. **Walk through the code**: Explain step-by-step what happens
4. **Highlight a gotcha**: What's a common misconception?

Keep explanations conversational. For complex concepts, use multiple analogies.
```


## 分配

- **项目技能**：将`.claude/skills/`提交给版本控制
- **插件**：将`skills/`目录添加到带有技能文件夹的插件中
- **企业**：通过托管设置在组织范围内部署