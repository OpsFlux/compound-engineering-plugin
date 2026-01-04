# 技能创作最佳实践

来源：[platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)

## 核心原则

### 简洁是关键

上下文窗口是一种公共物品。您的技能与克劳德需要了解的其他内容共享上下文窗口。

**默认假设**：克劳德已经非常聪明了。只添加克劳德还没有的上下文。

挑战每一条信息：
——“克劳德真的需要这个解释吗？”
- “我可以假设克劳德知道这一点吗？”
- “这一段是否证明其代币成本合理？”

**很好的例子（简洁，约 50 个标记）：**
```markdown
## Extract PDF text

Use pdfplumber for text extraction:

```python

导入 pdfplumber
使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```
```


**不好的例子（太冗长，约 150 个标记）：**
```markdown
## Extract PDF text

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available...
```


### 设置适当的自由度

将特异性与任务的脆弱性和可变性相匹配。

**高自由度**（多种有效方法）：
```markdown
## Code review process

1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability
4. Verify adherence to project conventions
```


**中等自由度**（具有变化的首选模式）：
```markdown
## Generate report

Use this template and customize as needed:

```python

defgenerate_report（数据，格式=“markdown”）：
    # 处理数据
    # 生成指定格式的输出
```
```


**低自由度**（脆弱，需要精确的顺序）：
```markdown
## Database migration

Run exactly this script:

```bash

python scripts/migrate.py --验证 --备份
```

Do not modify the command or add flags.
```


### 使用所有模型进行测试

技能充当模型的补充。使用俳句、十四行诗和作品进行测试。

- **俳句**：技能是否提供足够的指导？
- **十四行诗**：技能是否清晰高效？
- **Opus**：技能是否避免过度解释？

## 命名约定

使用**动名词形式**（动词 + -ing）作为技能名称：

**好：**
- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`
- `testing-code`
- `writing-documentation`

**可接受的替代方案：**
- 名词短语：`pdf-processing`、`spreadsheet-analysis`
- 以行动为导向：`process-pdfs`、`analyze-spreadsheets`

**避免：**
- 模糊：`helper`、`utils`、`tools`
- 通用：`documents`、`data`、`files`
- 保留：`anthropic-*`、`claude-*`

## 撰写有效的描述

**始终以第三人称书写。** 描述被注入到系统提示符中。

**具体并包括关键术语：**

```yaml
# PDF Processing skill
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.

# Excel Analysis skill
description: Analyze Excel spreadsheets, create pivot tables, generate charts. Use when analyzing Excel files, spreadsheets, tabular data, or .xlsx files.

# Git Commit Helper skill
description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.
```


**避免模糊的描述：**
```yaml
description: Helps with documents  # Too vague!
description: Processes data       # Too generic!
description: Does stuff with files # Useless!
```


## 渐进式披露模式

### 模式 1：带有参考资料的高级指南

```markdown
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, merges documents.
---

# PDF Processing

## Quick start

```python

导入 pdfplumber
使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```

## Advanced features

**Form filling**: See [FORMS.md](FORMS.md)
**API reference**: See [REFERENCE.md](REFERENCE.md)
**Examples**: See [EXAMPLES.md](EXAMPLES.md)
```


### 模式 2：特定领域的组织

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```


### 模式 3：条件细节

```markdown
# DOCX Processing

## Creating documents

Use docx-js for new documents. See [DOCX-JS.md](DOCX-JS.md).

## Editing documents

For simple edits, modify the XML directly.

**For tracked changes**: See [REDLINING.md](REDLINING.md)
**For OOXML details**: See [OOXML.md](OOXML.md)
```


## 将参考文献保留一层深度

当从其他引用的文件引用文件时，克劳德可能会部分读取文件。

**不好（太深）：**
```markdown
# SKILL.md
See [advanced.md](advanced.md)...

# advanced.md
See [details.md](details.md)...

# details.md
Here's the actual information...
```


**好（一层深）：**
```markdown
# SKILL.md

**Basic usage**: [in SKILL.md]
**Advanced features**: See [advanced.md](advanced.md)
**API reference**: See [reference.md](reference.md)
**Examples**: See [examples.md](examples.md)
```


## 工作流程和反馈循环

### 带清单的工作流程

```markdown
## Research synthesis workflow

Copy this checklist:

```

- [ ] 步骤1：阅读所有源文档
- [ ] 第 2 步：确定关键主题
- [ ] 步骤 3：交叉引用权利要求
- [ ] 步骤 4：创建结构化摘要
- [ ] 步骤 5：验证引文
```

**Step 1: Read all source documents**

Review each document in `sources/`. Note main arguments.
...
```


### 反馈循环模式

```markdown
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message
   - Fix the issues
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python scripts/pack.py unpacked_dir/ output.docx`
```


## 常见模式

### 模板模式

```markdown
## Report structure

Use this template:

```markdown

# [分析标题]

## 执行摘要
[一段概述]

## 主要发现
- 找到 1 并提供支持数据
- 发现 2 并提供支持数据

## 建议
1. 具体可行的建议
2. 具体可行的建议
```
```


### 示例模式

```markdown
## Commit message format

**Example 1:**
Input: Added user authentication with JWT tokens
Output:
```

feat(auth)：实现基于 JWT 的身份验证

添加登录端点和令牌验证中间件
```

**Example 2:**
Input: Fixed bug where dates displayed incorrectly
Output:
```

修复（报告）：时区转换中的正确日期格式
```
```


### 条件工作流模式

```markdown
## Document modification

1. Determine the modification type:

   **Creating new content?** → Follow "Creation workflow"
   **Editing existing?** → Follow "Editing workflow"

2. Creation workflow:
   - Use docx-js library
   - Build document from scratch

3. Editing workflow:
   - Unpack existing document
   - Modify XML directly
   - Validate after each change
```


## 内容指南

### 避免时间敏感的信息

**不好：**
```markdown
If you're doing this before August 2025, use the old API.
```


**好：**
```markdown
## Current method

Use the v2 API endpoint: `api.example.com/v2/messages`

## Old patterns

<details>
<summary>Legacy v1 API (deprecated 2025-08)</summary>
The v1 API used: `api.example.com/v1/messages`
</details>
```


### 使用一致的术语

**良好 - 一致：**
- 始终“API端点”
- 始终“田野”
- 始终“提取”

**不好 - 不一致：**
- 混合“API端点”，“URL”，“API路由”，“路径”
- 混合“字段”、“框”、“元素”、“控制”

## 要避免的反模式

### Windows 样式路径

- **好**：`scripts/helper.py`，`reference/guide.md`
- **避免**：`scripts\helper.py`、`reference\guide.md`

### 太多选择

**不好：**
```markdown
You can use pypdf, or pdfplumber, or PyMuPDF, or pdf2image, or...
```


**好：**
```markdown
Use pdfplumber for text extraction:
```python

导入 pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
```


## 有效技能清单

### 核心品质
- [ ] 描述具体并包含关键术语
- [ ] 描述包括内容和时间
- [ ] SKILL.md 正文 500 行以下
- [ ] 单独文件中的其他详细信息
- [ ] 没有时间敏感的信息
- [ ] 一致的术语
- [ ] 例子是具体的
- [ ] 引用一层深
- [ ] 适当使用渐进式披露
- [ ] 工作流程步骤清晰

### 代码和脚本
- [ ] 脚本明确处理错误
- [ ] 没有“巫毒常量”（所有值均合理）
- [ ] 列出所需的软件包
- [ ] 脚本有清晰的文档
- [ ] 没有 Windows 风格的路径
- [ ] 关键操作的验证步骤
- [ ] 质量关键任务的反馈循环

### 测试
- [ ] 至少三个测试场景
- [ ] 使用 Haiku、Sonnet 和 Opus 进行测试
- [ ] 经过真实使用场景测试
- [ ] 纳入团队反馈