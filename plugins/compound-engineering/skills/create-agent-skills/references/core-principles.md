<overview>
核心原则指导技能创作决策。这些原则确保技能在不同模型和用例中高效、有效且可维护。
</overview>

<xml_structure_principle>
<description>
技能使用纯 XML 结构来实现一致的解析、高效的令牌使用和改进的 Claude 性能。
</description>

<why_xml>
<consistency>
XML 在所有技能中强制实施一致的结构。所有技能都出于相同的目的使用相同的标签名称：
- `<objective>`始终定义技能的作用
- `<quick_start>`始终提供即时指导
- `<success_criteria>`始终定义完成

这种一致性使得技能可预测且更易于维护。
</consistency>

<parseability>
XML 提供了明确的边界和语义。克劳德可以可靠地：
- 确定部分边界（内容开始和结束的地方）
- 了解内容目的（每个部分扮演什么角色）
- 跳过不相关的部分（渐进式披露）
- 以编程方式解析（验证工具可以检查结构）

Markdown 标题只是视觉格式。克劳德必须从标题文本中推断含义，而这不太可靠。
</parseability>

<token_efficiency>
XML 标签比 Markdown 标题更有效：

**降价标题**：
```markdown
## Quick start
## Workflow
## Advanced features
## Success criteria
```

总计：约 20 个标记，对 Claude 来说没有语义意义

**XML 标签**：
```xml
<quick_start>
<workflow>
<advanced_features>
<success_criteria>
```

总计：~15 个标记，内置语义

生态系统中所有技能的储蓄复合。
</token_efficiency>

<claude_performance>
Claude 使用纯 XML 表现更好，因为：
- 明确的部分边界减少解析错误
- 语义标签直接传达意图（无需推理）
- 嵌套标签创建清晰的层次结构
- 跨技能的一致结构减少了认知负担
- 渐进式披露工作更可靠

纯 XML 结构不仅仅是一种风格偏好，它还是一种性能优化。
</claude_performance>
</why_xml>

<critical_rule>
**从技能正文内容中删除所有 Markdown 标题（#、##、###）。** 替换为语义 XML 标签。在内容中保持 Markdown 格式（粗体、斜体、列表、代码块、链接）。
</critical_rule>

<required_tags>
每项技能必须具备：
- `<objective>` - 该技能的作用及其重要性
- `<quick_start>` - 立即、可行的指导
- `<success_criteria>` 或 `<when_successful>` - 如何知道它有效

有关条件标签和智能规则，请参阅 [use-xml-tags.md](use-xml-tags.md)。
</required_tags>
</xml_structure_principle>

<conciseness_principle>
<description>
上下文窗口是共享的。您的技能与系统提示、对话历史记录、其他技能的元数据和实际请求共享。
</description>

<guidance>
只添加克劳德还没有的上下文。挑战每一条信息：
——“克劳德真的需要这个解释吗？”
- “我可以假设克劳德知道这一点吗？”
- “这一段是否证明其代币成本合理？”

假设克劳德很聪明。不要解释明显的概念。
</guidance>

<concise_example>
**简洁**（约 50 个标记）：
```xml
<quick_start>
Extract PDF text with pdfplumber:

```python

导入 pdfplumber

使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```
</quick_start>
```


**详细**（约 150 个标记）：
```xml
<quick_start>
PDF files are a common file format used for documents. To extract text from them, we'll use a Python library called pdfplumber. First, you'll need to import the library, then open the PDF file using the open method, and finally extract the text from each page. Here's how to do it:

```python

导入 pdfplumber

使用 pdfplumber.open("file.pdf") 作为 pdf：
    文本 = pdf.pages[0].extract_text()
```

This code opens the PDF and extracts text from the first page.
</quick_start>
```


简洁版本假设 Claude 知道 PDF 是什么、理解 Python 导入并且可以阅读代码。所有这些假设都是正确的。
</concise_example>

<when_to_elaborate>
添加说明：
- 概念是特定领域的（不是一般编程知识）
- 模式不明显或违反直觉
- 环境以微妙的方式影响行为
- 权衡需要判断

不要添加以下内容的解释：
- 常见编程概念（循环、函数、导入）
- 标准库的使用（读取文件、发出 HTTP 请求）
- 知名工具（git、npm、pip）
- 明显的后续步骤
</when_to_elaborate>
</conciseness_principle>

<degrees_of_freedom_principle>
<description>
将具体程度与任务的脆弱性和可变性相匹配。给予克劳德更多的自由来执行创造性任务，减少脆弱操作的自由。
</description>

<high_freedom>
<when>
- 多种方法均有效
- 决定取决于具体情况
- 启发式指导方法
- 欢迎创意解决方案
</when>

<example>
```xml
<objective>
Review code for quality, bugs, and maintainability.
</objective>

<workflow>
1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions
</workflow>

<success_criteria>
- All major issues identified
- Suggestions are actionable and specific
- Review balances praise and criticism
</success_criteria>
```


克劳德可以根据代码的需要自由地调整审查。
</example>
</high_freedom>

<medium_freedom>
<when>
- 存在首选模式
- 一些变化是可以接受的
- 配置影响行为
- 模板可以调整
</when>

<example>
```xml
<objective>
Generate reports with customizable format and sections.
</objective>

<report_template>
Use this template and customize as needed:

```python

defgenerate_report（数据，格式=“markdown”，include_charts=True）：
    # 处理数据
    # 生成指定格式的输出
    # 可选择包含可视化
```
</report_template>

<success_criteria>
- Report includes all required sections
- Format matches user preference
- Data accurately represented
</success_criteria>
```


Claude可以根据需求定制模板。
</example>
</medium_freedom>

<low_freedom>
<when>
- 操作脆弱且容易出错
- 一致性至关重要
- 必须遵循特定的顺序
- 偏差导致故障
</when>

<example>
```xml
<objective>
Run database migration with exact sequence to prevent data loss.
</objective>

<workflow>
Run exactly this script:

```bash

python scripts/migrate.py --验证 --备份
```

**Do not modify the command or add additional flags.**
</workflow>

<success_criteria>
- Migration completes without errors
- Backup created before migration
- Verification confirms data integrity
</success_criteria>
```


克劳德必须严格执行命令，不得有任何变化。
</example>
</low_freedom>

<matching_specificity>
关键是将特异性与脆弱性相匹配：

- **脆弱的操作**（数据库迁移、支付处理、安全）：自由度低，指令准确
- **标准操作**（API调用、文件处理、数据转换）：中等自由度，具有灵活性的首选模式
- **创意运营**（代码审查、内容生成、分析）：高度自由、启发式和原则

不匹配的特异性会导致问题：
- 在脆弱的任务上有太多的自由→错误和失败
- 创造性任务的自由度太低 → 僵化的、次优的产出
</matching_specificity>
</degrees_of_freedom_principle>

<model_testing_principle>
<description>
技能充当模型的补充，因此有效性取决于底层模型。适用于 Opus 的内容可能需要更多适用于 Haiku 的细节。
</description>

<testing_across_models>
使用您计划使用的所有模型测试您的技能：

<haiku_testing>
**克劳德俳句**（快速、经济）

要问的问题：
- 该技能是否提供足够的指导？
- 例子是否清晰、完整？
- 隐含的假设是否变得明确？
- 俳句需要更多的结构吗？

俳句的好处是：
- 更明确的指示
- 完整的示例（没有部分代码）
- 明确的成功标准
- 分步工作流程
</haiku_testing>

<sonnet_testing>
**克劳德十四行诗**（平衡）

要问的问题：
- 技能是否清晰、高效？
- 是否避免过度解释？
- 工作流程结构是否良好？
- 渐进式披露有效吗？

十四行诗的好处是：
- 平衡的细节水平
- XML 结构清晰
- 渐进式披露
- 简洁但完整的指导
</sonnet_testing>

<opus_testing>
**克劳德·奥普斯**（强大的推理）

要问的问题：
- 技能是否避免过度解释？
- Opus 可以推断出明显的步骤吗？
- 限制条件是否明确？
- 上下文是否最少但足够？

Opus 的优势在于：
- 简洁说明
- 原则重于程序
- 高自由度
- 相信推理能力
</opus_testing>
</testing_across_models>

<balancing_across_models>
旨在获得适用于所有目标模型的说明：

**良好的平衡**：
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


这适用于所有型号：
- Haiku 获得完整的工作示例
- Sonnet 通过逃生舱口获得明确的默认设置
- Opus 获得了足够的背景信息，没有过度解释

**对于俳句来说太少了**：
```xml
<quick_start>
Use pdfplumber for text extraction.
</quick_start>
```


**对于 Opus 来说太冗长**：
```xml
<quick_start>
PDF files are documents that contain text. To extract that text, we use a library called pdfplumber. First, import the library at the top of your Python file. Then, open the PDF file using the pdfplumber.open() method. This returns a PDF object. Access the pages attribute to get a list of pages. Each page has an extract_text() method that returns the text content...
</quick_start>
```

</balancing_across_models>

<iterative_improvement>
1. 从中等细节级别开始
2. 使用目标模型进行测试
3. 观察模型在哪些方面挣扎或成功
4、根据实际表现进行调整
5. 重新测试和迭代

不要针对一种模型进行优化。找到适合您的目标模型的平衡点。
</iterative_improvement>
</model_testing_principle>

<progressive_disclosure_principle>
<description>
SKILL.md 用作概述。参考文件包含详细信息。克劳德仅在需要时加载参考文件。
</description>

<token_efficiency>
渐进式披露使代币使用量与任务复杂性成正比：

- 简单任务：仅加载 SKILL.md（约 500 个令牌）
- 中等任务：加载 SKILL.md + 一份参考资料（约 1000 个令牌）
- 复杂任务：加载 SKILL.md + 多个引用（~2000 个令牌）

如果没有渐进式披露，每个任务都会加载所有内容，无论需要如何。
</token_efficiency>

<implementation>
- 将 SKILL.md 控制在 500 行以下
- 将详细内容拆分为参考文件
- 将 SKILL.md 的参考文献保留一层深
- 相关部分参考文献的链接
- 使用描述性参考文件名

请参阅 [skill-struct.md](skill-struct.md) 了解渐进式披露模式。
</implementation>
</progressive_disclosure_principle>

<validation_principle>
<description>
验证脚本是力量倍增器。他们发现克劳德可能错过的错误并提供可操作的反馈。
</description>

<characteristics>
好的验证脚本：
- 提供详细、具体的错误消息
- 当某些内容无效时显示可用的有效选项
- 查明问题的确切位置
- 建议可行的修复措施
- 确定性且可靠

请参阅 [workflows-and-validation.md](workflows-and-validation.md) 了解验证模式。
</characteristics>
</validation_principle>

<principle_summary>
<xml_structure>
使用纯 XML 结构来实现一致性、可解析性和 Claude 性能。必需的标签：objective、quick_start、success_criteria。
</xml_structure>

<conciseness>
只添加克劳德没有的上下文。假设克劳德很聪明。挑战每一项内容。
</conciseness>

<degrees_of_freedom>
将特异性与脆弱性相匹配。创造性任务的自由度高，脆弱操作的自由度低，标准工作的自由度中等。
</degrees_of_freedom>

<model_testing>
使用所有目标模型进行测试。平衡俳句、十四行诗和作品的细节水平。
</model_testing>

<progressive_disclosure>
保持 SKILL.md 简洁。将详细信息拆分为参考文件。仅在需要时加载参考文件。
</progressive_disclosure>

<validation>
使验证脚本冗长且具体。通过可操作的反馈尽早发现错误。
</validation>
</principle_summary>