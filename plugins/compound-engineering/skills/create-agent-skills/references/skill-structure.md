<overview>
技能具有三个结构组成部分：YAML frontmatter（元数据）、纯 XML 主体结构（内容组织）和渐进式披露（文件组织）。该参考定义了每个组件的要求和最佳实践。
</overview>

<xml_structure_requirements>
<critical_rule>
**从技能正文内容中删除所有 Markdown 标题（#、##、###）。** 替换为语义 XML 标签。在内容中保持 Markdown 格式（粗体、斜体、列表、代码块、链接）。
</critical_rule>

<required_tags>
每个技能都必须具有这三个标签：

- **`<objective>`** - 该技能的作用及其重要性（1-3 段）
- **`<quick_start>`** - 即时、可操作的指导（最小工作示例）
- **`<success_criteria>`** 或 **`<when_successful>`** - 如何知道它有效
</required_tags>

<conditional_tags>
根据技能复杂性和领域要求添加：

- **`<context>`** - 背景/情况信息
- **`<workflow>` 或 `<process>`** - 分步程序
- **`<advanced_features>`** - 深入主题（逐步披露）
- **`<validation>`** - 如何验证输出
- **`<examples>`** - 多次学习
- **`<anti_patterns>`** - 要避免的常见错误
- **`<security_checklist>`** - 不可协商的安全模式
- **`<testing>`** - 测试工作流程
- **`<common_patterns>`** - 代码示例和配方
- **`<reference_guides>` 或 `<detailed_references>`** - 参考文件链接

有关每个标签的详细指南，请参阅 [use-xml-tags.md](use-xml-tags.md)。
</conditional_tags>

<tag_selection_intelligence>
**简单技巧**（单一领域，简单明了）：
- 仅必需的标签
- 示例：文本提取、文件格式转换

**中等技能**（多种模式，一些复杂性）：
- 所需的标签+工作流程/示例（根据需要）
- 示例：文档处理步骤、API 集成

**复杂技能**（多域、安全性、API）：
- 必要标签+适当的条件标签
- 示例：支付处理、身份验证系统、多步骤工作流程
</tag_selection_intelligence>

<xml_nesting>
为分层内容正确嵌套 XML 标签：

```xml
<examples>
<example number="1">
<input>User input</input>
<output>Expected output</output>
</example>
</examples>
```


始终关闭标签：
```xml
<objective>
Content here
</objective>
```

</xml_nesting>

<tag_naming_conventions>
使用描述性的语义名称：
- `<workflow>` 不是`<steps>`
- `<success_criteria>` 不是`<done>`
- `<anti_patterns>` 不是`<dont_do>`

在你的技能范围内保持一致。如果您使用 `<workflow>`，请勿将 `<process>` 也用于同一目的（除非它们发挥不同的作用）。
</tag_naming_conventions>
</xml_structure_requirements>

<yaml_requirements>
<required_fields>
```yaml
---
name: skill-name-here
description: What it does and when to use it (third person, specific triggers)
---
```

</required_fields>

<name_field>
**验证规则**：
- 最多 64 个字符
- 仅限小写字母、数字、连字符
- 没有 XML 标签
- 无保留字：“anthropic”、“claude”
- 必须完全匹配目录名称

**示例**：
- ✅`process-pdfs`
- ✅`manage-facebook-ads`
- ✅`setup-stripe-payments`
- ❌`PDF_Processor`（大写）
- ❌`helper`（含糊）
- ❌`claude-helper`（保留字）
</name_field>

<description_field>
**验证规则**：
- 非空，最多 1024 个字符
- 没有 XML 标签
- 第三人称（绝不是第一人称或第二人称）
- 包括它的作用和何时使用它

**关键规则**：始终以第三人称书写。
- ✅“处理 Excel 文件并生成报告”
- ❌“我可以帮你处理Excel文件”
- ❌“您可以使用它来处理Excel文件”

**结构**：包括功能和触发器。

**有效例子**：
```yaml
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```


```yaml
description: Analyze Excel spreadsheets, create pivot tables, generate charts. Use when analyzing Excel files, spreadsheets, tabular data, or .xlsx files.
```


```yaml
description: Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes.
```


**避免**：
```yaml
description: Helps with documents
```


```yaml
description: Processes data
```

</description_field>
</yaml_requirements>

<naming_conventions>
使用**动词-名词约定**作为技能名称：

<pattern name="create">
构建/创作工具

示例：`create-agent-skills`、`create-hooks`、`create-landing-pages`
</pattern>

<pattern name="manage">
管理外部服务或资源

示例：`manage-facebook-ads`、`manage-zoom`、`manage-stripe`、`manage-supabase`
</pattern>

<pattern name="setup">
配置/集成任务

示例：`setup-stripe-payments`、`setup-meta-tracking`
</pattern>

<pattern name="generate">
生成任务

示例：`generate-ai-images`
</pattern>

<avoid_patterns>
- 模糊：`helper`、`utils`、`tools`
- 通用：`documents`、`data`、`files`
- 保留字：`anthropic-helper`、`claude-tools`
- 不一致：目录`facebook-ads`但名称`facebook-ads-manager`
</avoid_patterns>
</naming_conventions>

<progressive_disclosure>
<principle>
SKILL.md 充当概述，根据需要指向详细材料。这可以保持上下文窗口的高效使用。
</principle>

<practical_guidance>
- 将 SKILL.md 正文控制在 500 行以下
- 当接近此限制时，将内容拆分为单独的文件
- 将 SKILL.md 的参考文献保留一层深
- 将目录添加到超过 100 行的参考文件
</practical_guidance>

<pattern name="high_level_guide">
SKILL.md 中的快速入门，参考文件中的详细信息：

```markdown
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, and merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---

<objective>
Extract text and tables from PDF files, fill forms, and merge documents using Python libraries.
</objective>

<quick_start>
Extract text with pdfplumber:

```python

导入 pdfplumber
使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```
</quick_start>

<advanced_features>
**Form filling**: See [forms.md](forms.md)
**API reference**: See [reference.md](reference.md)
</advanced_features>
```


Claude仅在需要时加载forms.md或reference.md。
</pattern>

<pattern name="domain_organization">
对于具有多个领域的技能，请按领域进行组织以避免加载不相关的上下文：

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```


当用户询问收入时，Claude 只阅读 Finance.md。其他文件保留在消耗零令牌的文件系统上。
</pattern>

<pattern name="conditional_details">
在SKILL.md中显示基本内容，链接到参考文件中的高级内容：

```xml
<objective>
Process DOCX files with creation and editing capabilities.
</objective>

<quick_start>
<creating_documents>
Use docx-js for new documents. See [docx-js.md](docx-js.md).
</creating_documents>

<editing_documents>
For simple edits, modify XML directly.

**For tracked changes**: See [redlining.md](redlining.md)
**For OOXML details**: See [ooxml.md](ooxml.md)
</editing_documents>
</quick_start>
```


仅当用户需要这些功能时，Claude 才会读取 redlined.md 或 ooxml.md。
</pattern>

<critical_rules>
**将参考保持一层深度**：所有参考文件应直接从 SKILL.md 链接。避免嵌套引用（SKILL.md→advanced.md→details.md），因为 Claude 可能只能部分读取深度嵌套的文件。

**向长文件添加目录**：对于超过 100 行的参考文件，请在顶部添加目录。

**在参考文件中使用纯 XML**：参考文件也应该使用纯 XML 结构（正文中没有 Markdown 标题）。
</critical_rules>
</progressive_disclosure>

<file_organization>
<filesystem_navigation>
Claude 使用 bash 命令导航您的技能目录：

- 使用正斜杠：`reference/guide.md`（不是`reference\guide.md`）
- 描述性地命名文件：`form_validation_rules.md`（不是`doc2.md`）
- 按域组织：`reference/finance.md`、`reference/sales.md`
</filesystem_navigation>

<directory_structure>
典型的技能结构：

```
skill-name/
├── SKILL.md (main entry point, pure XML structure)
├── references/ (optional, for progressive disclosure)
│   ├── guide-1.md (pure XML structure)
│   ├── guide-2.md (pure XML structure)
│   └── examples.md (pure XML structure)
└── scripts/ (optional, for utility scripts)
    ├── validate.py
    └── process.py
```

</directory_structure>
</file_organization>

<anti_patterns>
<pitfall name="markdown_headings_in_body">
❌ 不要在技能正文中使用 Markdown 标题：

```markdown
# PDF Processing

## Quick start
Extract text...

## Advanced features
Form filling...
```


✅ 使用纯XML结构：

```xml
<objective>
PDF processing with text extraction, form filling, and merging.
</objective>

<quick_start>
Extract text...
</quick_start>

<advanced_features>
Form filling...
</advanced_features>
```

</pitfall>

<pitfall name="vague_descriptions">
- ❌“帮助处理文档”
- ✅“从 PDF 文件中提取文本和表格、填写表单、合并文档。在处理 PDF 文件或用户提及 PDF、表单或文档提取时使用。”
</pitfall>

<pitfall name="inconsistent_pov">
- ❌“我可以帮你处理Excel文件”
- ✅“处理 Excel 文件并生成报告”
</pitfall>

<pitfall name="wrong_naming_convention">
- ❌目录：`facebook-ads`，名称：`facebook-ads-manager`
- ✅ 目录：`manage-facebook-ads`，名称：`manage-facebook-ads`
- ❌目录：`stripe-integration`，名称：`stripe`
- ✅ 目录：`setup-stripe-payments`，名称：`setup-stripe-payments`
</pitfall>

<pitfall name="deeply_nested_references">
将 SKILL.md 中的引用保持深一层。 Claude 只能部分读取嵌套文件（SKILL.md→advanced.md→details.md）。
</pitfall>

<pitfall name="windows_paths">
始终使用正斜杠：`scripts/helper.py`（不是`scripts\helper.py`）
</pitfall>

<pitfall name="missing_required_tags">
每项技能都必须具有：`<objective>`、`<quick_start>` 和 `<success_criteria>`（或`<when_successful>`）。
</pitfall>
</anti_patterns>

<validation_checklist>
在最终确定技能之前，请验证：

- ✅ YAML frontmatter 有效（名称与目录匹配，第三人称描述）
- ✅ 正文中没有 Markdown 标题（纯 XML 结构）
- ✅ 存在必需的标签：objective、quick_start、success_criteria
- ✅适合复杂程度的条件标签
- ✅ 所有 XML 标签都正确关闭
- ✅ 应用渐进式披露（SKILL.md < 500 lines)
- ✅ Reference files use pure XML structure
- ✅ File paths use forward slashes
- ✅ Descriptive file names
</validation_checklist>