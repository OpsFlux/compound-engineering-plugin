<overview>
本参考记录了技能创作的常见模式，包括模板、示例、术语一致性和反模式。所有模式都使用纯 XML 结构。
</overview>

<template_pattern>
<description>
提供输出格式的模板。将严格程度与您的需求相匹配。
</description>

<strict_requirements>
当输出格式必须准确且一致时使用：

```xml
<report_structure>
ALWAYS use this exact template structure:

```markdown

# [分析标题]

## 执行摘要
[主要发现的一段概述]

## 主要发现
- 找到 1 并提供支持数据
- 发现 2 并提供支持数据
- 发现 3 并提供支持数据

## 建议
1. 具体可行的建议
2. 具体可行的建议
```
</report_structure>
```


**何时使用**：合规报告、标准化格式、自动化处理
</strict_requirements>

<flexible_guidance>
当 Claude 应根据上下文调整格式时使用：

```xml
<report_structure>
Here is a sensible default format, but use your best judgment:

```markdown

# [分析标题]

## 执行摘要
[概述]

## 主要发现
[根据您的发现调整部分]

## 建议
【根据具体情况量身定制】
```

Adjust sections as needed for the specific analysis type.
</report_structure>
```


**何时使用**：探索性分析、上下文相关格式、创意任务
</flexible_guidance>
</template_pattern>

<examples_pattern>
<description>
对于输出质量取决于示例的技能，请提供输入/输出对。
</description>

<commit_messages_example>
```xml
<objective>
Generate commit messages following conventional commit format.
</objective>

<commit_message_format>
Generate commit messages following these examples:

<example number="1">
<input>Added user authentication with JWT tokens</input>
<output>
```

feat(auth)：实现基于 JWT 的身份验证

添加登录端点和令牌验证中间件
```
</output>
</example>

<example number="2">
<input>Fixed bug where dates displayed incorrectly in reports</input>
<output>
```

修复（报告）：时区转换中的正确日期格式

在报告生成过程中一致使用 UTC 时间戳
```
</output>
</example>

Follow this style: type(scope): brief description, then detailed explanation.
</commit_message_format>
```

</commit_messages_example>

<when_to_use>
- 输出格式存在文本解释无法捕获的细微差别
- 模式识别比规则遵循更容易
- 示例展示了边缘情况
- 多次学习提高质量
</when_to_use>
</examples_pattern>

<consistent_terminology>
<principle>
选择一个术语并在整个技能中使用它。不一致的术语会让克劳德感到困惑并降低执行质量。
</principle>

<good_example>
一致的用法：
- 始终为“API端点”（不与“URL”、“API路由”、“路径”混合）
- 始终为“字段”（不与“框”、“元素”、“控件”混合）
- 始终“提取”（不要与“拉”、“获取”、“检索”混合）

```xml
<objective>
Extract data from API endpoints using field mappings.
</objective>

<quick_start>
1. Identify the API endpoint
2. Map response fields to your schema
3. Extract field values
</quick_start>
```

</good_example>

<bad_example>
不一致的用法会造成混乱：

```xml
<objective>
Pull data from API routes using element mappings.
</objective>

<quick_start>
1. Identify the URL
2. Map response boxes to your schema
3. Retrieve control values
</quick_start>
```


Claude 现在必须解释：“API 路由”和“URL”相同吗？ “字段”、“框”、“元素”和“控件”相同吗？
</bad_example>

<implementation>
1. 在技能开发早期选择术语
2. 记录 `<objective>` 或 `<context>` 中的关键术语
3. 使用查找/替换来强制一致性
4. 检查参考文件以确保用法一致
</implementation>
</consistent_terminology>

<provide_default_with_escape_hatch>
<principle>
为特殊情况提供带有逃生舱口的默认方法，而不是替代方案列表。太多的选择会阻碍决策。
</principle>

<good_example>
使用逃生舱口清除默认值：

```xml
<quick_start>
Use pdfplumber for text extraction:

```python

导入 pdfplumber
使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
</quick_start>
```

</good_example>

<bad_example>
太多的选择会导致决策瘫痪：

```xml
<quick_start>
You can use any of these libraries:

- **pypdf**: Good for basic extraction
- **pdfplumber**: Better for tables
- **PyMuPDF**: Faster but more complex
- **pdf2image**: For scanned documents
- **pdfminer**: Low-level control
- **tabula-py**: Table-focused

Choose based on your needs.
</quick_start>
```


克劳德现在必须在开始之前研究和比较所有选项。这浪费了代币和时间。
</bad_example>

<implementation>
1. 推荐一种默认方法
2. 解释何时使用默认值（隐含：大部分时间）
3.为边缘情况添加一个逃生舱口
4. 如果确实需要多种替代方案，请链接到高级参考
</implementation>
</provide_default_with_escape_hatch>

<anti_patterns>
<description>
创作技巧时要避免的常见错误。
</description>

<pitfall name="markdown_headings_in_body">
❌ **不好**：在技能正文中使用 Markdown 标题：

```markdown
# PDF Processing

## Quick start
Extract text with pdfplumber...

## Advanced features
Form filling requires additional setup...
```


✅ **好**：使用纯 XML 结构：

```xml
<objective>
PDF processing with text extraction, form filling, and merging capabilities.
</objective>

<quick_start>
Extract text with pdfplumber...
</quick_start>

<advanced_features>
Form filling requires additional setup...
</advanced_features>
```


**为什么重要**：XML 提供语义、可靠的解析和标记效率。
</pitfall>

<pitfall name="vague_descriptions">
❌ **不好**：
```yaml
description: Helps with documents
```


✅ **好**：
```yaml
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```


**为什么重要**：模糊的描述使克劳德无法正确发现和使用该技能。
</pitfall>

<pitfall name="inconsistent_pov">
❌ **不好**：
```yaml
description: I can help you process Excel files and generate reports
```


✅ **好**：
```yaml
description: Processes Excel files and generates reports. Use when analyzing spreadsheets or .xlsx files.
```


**为什么重要**：技能必须使用第三人称。第一/第二人称打破了技能元数据模式。
</pitfall>

<pitfall name="wrong_naming_convention">
❌ **BAD**：目录名称与技能名称或动词名词约定不匹配：
- 目录：`facebook-ads`，名称：`facebook-ads-manager`
- 目录：`stripe-integration`，名称：`stripe`
- 目录：`helper-scripts`，名称：`helper`

✅ **好**：一致的动词-名词约定：
- 目录：`manage-facebook-ads`，名称：`manage-facebook-ads`
- 目录：`setup-stripe-payments`，名称：`setup-stripe-payments`
- 目录：`process-pdfs`，名称：`process-pdfs`

**为什么重要**：命名的一致性使技能可被发现和可预测。
§§PH100​​§§

<pitfall name="too_many_options">
❌ **不好**：
```xml
<quick_start>
You can use pypdf, or pdfplumber, or PyMuPDF, or pdf2image, or pdfminer, or tabula-py...
</quick_start>
```


✅ **好**：
```xml
<quick_start>
Use pdfplumber for text extraction:

```python

导入 pdfplumber
```

For scanned PDFs requiring OCR, use pdf2image with pytesseract instead.
</quick_start>
```


**为什么重要**：决策瘫痪。为特殊情况提供一种带有逃生舱口的默认方法。
</pitfall>

<pitfall name="deeply_nested_references">
❌ **BAD**：引用嵌套多个级别：
```
SKILL.md → advanced.md → details.md → examples.md
```


✅ **好**：引用了 SKILL.md 的深一层：
```
SKILL.md → advanced.md
SKILL.md → details.md
SKILL.md → examples.md
```


**为什么重要**：Claude 可能只能部分读取深度嵌套的文件。将 SKILL.md 中的引用保持深一层。
</pitfall>

<pitfall name="windows_paths">
❌ **不好**：
```xml
<reference_guides>
See scripts\validate.py for validation
</reference_guides>
```


✅ **好**：
```xml
<reference_guides>
See scripts/validate.py for validation
</reference_guides>
```


**为什么重要**：始终使用正斜杠来实现跨平台兼容性。
</pitfall>

<pitfall name="dynamic_context_and_file_reference_execution">
**问题**：当显示动态上下文语法（感叹号 + 反引号）或文件引用（@ 前缀）的示例时，技能加载器会在技能加载期间执行这些示例。

❌ **BAD** - 这些在技能加载期间执行：
```xml
<examples>
Load current status with: !`git status`
Review dependencies in: @package.json
</examples>
```


✅ **好** - 添加空间以防止执行：
```xml
<examples>
Load current status with: ! `git status` (remove space before backtick in actual usage)
Review dependencies in: @ package.json (remove space after @ in actual usage)
</examples>
```


**当适用时**：
- 向用户传授动态上下文的技能（斜线命令、提示）
- 显示感叹号前缀语法或@文件引用的任何文档
- 具有在加载期间不应执行的示例命令或文件路径的技能

**为什么重要**：如果没有空间，它们会在技能加载期间执行，从而导致错误或不需要的文件读取。
</pitfall>

<pitfall name="missing_required_tags">
❌ **不好**：缺少必需的标签：
```xml
<quick_start>
Use this tool for processing...
</quick_start>
```


✅ **好**：所有必需的标签都存在：
```xml
<objective>
Process data files with validation and transformation.
</objective>

<quick_start>
Use this tool for processing...
</quick_start>

<success_criteria>
- Input file successfully processed
- Output file validates without errors
- Transformation applied correctly
</success_criteria>
```


**为什么重要**：每项技能都必须具有`<objective>`、`<quick_start>`和`<success_criteria>`（或`<when_successful>`）。
</pitfall>

<pitfall name="hybrid_xml_markdown">
❌ **BAD**：将 XML 标签与 Markdown 标题混合：
```markdown
<objective>
PDF processing capabilities
</objective>

## Quick start

Extract text with pdfplumber...

## Advanced features

Form filling...
```


✅ **好**：纯 XML 贯穿始终：
```xml
<objective>
PDF processing capabilities
</objective>

<quick_start>
Extract text with pdfplumber...
</quick_start>

<advanced_features>
Form filling...
</advanced_features>
```


**为什么重要**：结构的一致性。使用纯 XML 或纯 Markdown（首选 XML）。
</pitfall>

<pitfall name="unclosed_xml_tags">
❌ **BAD**：忘记关闭 XML 标签：
```xml
<objective>
Process PDF files

<quick_start>
Use pdfplumber...
</quick_start>
```


✅ **好**：正确关闭的标签：
```xml
<objective>
Process PDF files
</objective>

<quick_start>
Use pdfplumber...
</quick_start>
```


**为什么重要**：未封闭的标签会破坏 XML 解析并创建不明确的边界。
</pitfall>
</anti_patterns>

<progressive_disclosure_pattern>
<description>
通过链接到详细的参考文件来保持 SKILL.md 的简洁。克劳德仅在需要时加载参考文件。
</description>

<implementation>
```xml
<objective>
Manage Facebook Ads campaigns, ad sets, and ads via the Marketing API.
</objective>

<quick_start>
<basic_operations>
See [basic-operations.md](basic-operations.md) for campaign creation and management.
</basic_operations>
</quick_start>

<advanced_features>
**Custom audiences**: See [audiences.md](audiences.md)
**Conversion tracking**: See [conversions.md](conversions.md)
**Budget optimization**: See [budgets.md](budgets.md)
**API reference**: See [api-reference.md](api-reference.md)
</advanced_features>
```


**好处**：
- SKILL.md 保持在 500 行以下
- 克劳德只阅读相关参考文件
- 令牌的使用随任务复杂性而变化
- 更容易维护和更新
</implementation>
</progressive_disclosure_pattern>

<validation_pattern>
<description>
对于验证步骤的技能，请使验证脚本变得详细且具体。
</description>

<implementation>
```xml
<validation>
After making changes, validate immediately:

```bash

蟒蛇scripts/validate.py输出目录/
```

If validation fails, fix errors before continuing. Validation errors include:

- **Field not found**: "Field 'signature_date' not found. Available fields: customer_name, order_total, signature_date_signed"
- **Type mismatch**: "Field 'order_total' expects number, got string"
- **Missing required field**: "Required field 'customer_name' is missing"

Only proceed when validation passes with zero errors.
</validation>
```


**为什么详细错误有帮助**：
- 克劳德可以在不猜测的情况下解决问题
- 特定的错误消息减少迭代周期
- 错误消息中显示可用选项
</implementation>
</validation_pattern>

<checklist_pattern>
<description>
对于复杂的多步骤工作流程，请提供克劳德可以复制和跟踪进度的清单。
</description>

<implementation>
```xml
<workflow>
Copy this checklist and check off items as you complete them:

```

任务进展：
- [ ] 步骤1：分析表单（运行analyze_form.py）
- [ ] 步骤 2：创建字段映射（编辑 fields.json）
- [ ] 步骤 3：验证映射（运行 validate_fields.py）
- [ ] 第 4 步：填写表格（运行 fill_form.py）
- [ ] 步骤 5：验证输出（运行 verify_output.py）
```

<step_1>
**Analyze the form**

Run: `python scripts/analyze_form.py input.pdf`

This extracts form fields and their locations, saving to `fields.json`.
</step_1>

<step_2>
**Create field mapping**

Edit `fields.json` to add values for each field.
</step_2>

<step_3>
**Validate mapping**

Run: `python scripts/validate_fields.py fields.json`

Fix any validation errors before continuing.
</step_3>

<step_4>
**Fill the form**

Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`
</step_4>

<step_5>
**Verify output**

Run: `python scripts/verify_output.py output.pdf`

If verification fails, return to Step 2.
</step_5>
</workflow>
```


**好处**：
- 清晰的进度跟踪
- 防止跳过步骤
- 中断后轻松恢复
</implementation>
</checklist_pattern>