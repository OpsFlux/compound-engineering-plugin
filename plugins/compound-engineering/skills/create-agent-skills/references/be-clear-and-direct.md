<golden_rule>
向背景很少的人展示您的技能，并要求他们按照说明进行操作。如果他们感到困惑，克劳德很可能也会感到困惑。
</golden_rule>

<overview>
清晰和直接是有效技能创作的基础。清晰的指令可以减少错误，提高执行质量，并最大限度地减少令牌浪费。
</overview>

<guidelines>
<contextual_information>
为克劳德提供框架任务的上下文信息：

- 任务结果的用途
- 输出的目标受众是什么
- 该任务属于哪个工作流程
- 最终目标或成功完成的样子

上下文帮助克劳德做出更好的决策并产生更合适的输出。

<example>
```xml
<context>
This analysis will be presented to investors who value transparency and actionable insights. Focus on financial metrics and clear recommendations.
</context>
```

</example>
</contextual_information>

<specificity>
具体说明您希望克劳德做什么。如果您只想要代码而不需要其他任何东西，请直接说出来。

**含糊**：“帮助报告”
**具体**：“生成包含三个部分的降价报告：执行摘要、主要发现、建议”

**含糊**：“处理数据”
**具体**：“从 CSV 文件中提取客户姓名和电子邮件地址，删除重复项，然后保存为 JSON 格式”

特异性消除了歧义并减少了迭代周期。
</specificity>

<sequential_steps>
以连续步骤的形式提供说明。使用编号列表或项目符号点。

```xml
<workflow>
1. Extract data from source file
2. Transform to target format
3. Validate transformation
4. Save to output file
5. Verify output correctness
</workflow>
```


连续的步骤可以创建明确的期望并减少 Claude 跳过重要操作的机会。
</sequential_steps>
</guidelines>

<example_comparison>
<unclear_example>
```xml
<quick_start>
Please remove all personally identifiable information from these customer feedback messages: {{FEEDBACK_DATA}}
</quick_start>
```


**问题**：
- 什么算作 PII？
- 什么应该取代 PII？
- 输出应该是什么格式？
- 如果没有找到 PII 怎么办？
- 产品名称是否应该被编辑？
</unclear_example>

<clear_example>
```xml
<objective>
Anonymize customer feedback for quarterly review presentation.
</objective>

<quick_start>
<instructions>
1. Replace all customer names with "CUSTOMER_[ID]" (e.g., "Jane Doe" → "CUSTOMER_001")
2. Replace email addresses with "EMAIL_[ID]@example.com"
3. Redact phone numbers as "PHONE_[ID]"
4. If a message mentions a specific product (e.g., "AcmeCloud"), leave it intact
5. If no PII is found, copy the message verbatim
6. Output only the processed messages, separated by "---"
</instructions>

Data to process: {{FEEDBACK_DATA}}
</quick_start>

<success_criteria>
- All customer names replaced with IDs
- All emails and phones redacted
- Product names preserved
- Output format matches specification
</success_criteria>
```


**为什么这样更好**：
- 说明目的（季度审查）
- 提供明确的分步规则
- 明确定义输出格式
- 指定边缘情况（产品名称，未找到 PII）
- 定义成功标准
</clear_example>
</example_comparison>

<key_differences>
清晰版：
- 说明目的（季度审查）
- 提供明确的分步规则
- 定义输出格式
- 指定边缘情况（产品名称，未找到 PII）
- 包括成功标准

不清楚的版本将所有这些决定留给了克劳德，增加了与预期不一致的可能性。
</key_differences>

<show_dont_just_tell>
<principle>
当格式很重要时，请展示一个示例，而不仅仅是描述它。
</principle>

<telling_example>
```xml
<commit_messages>
Generate commit messages in conventional format with type, scope, and description.
</commit_messages>
```

</telling_example>

<showing_example>
```xml
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

</showing_example>

<why_showing_works>
示例传达了文本描述无法传达的细微差别：
- 精确的格式（空格、大写、标点符号）
- 语气和风格
- 详细程度
- 跨多种情况的模式

克劳德从示例中学习模式比从描述中学习模式更可靠。
</why_showing_works>
</show_dont_just_tell>

<avoid_ambiguity>
<principle>
消除造成歧义或留下未决决定的单词和短语。
</principle>

<ambiguous_phrases>
❌ **“尝试...”** - 表示可选
✅ **“总是...”** 或 **“从不...”** - 明确的要求

❌ **“应该可能......”** - 义务不明确
✅ **“必须...”** 或 **“可以选择...”** - 明确的义务级别

❌ **“通常......”** - 什么时候允许例外？
✅ **“总是......除非......”** - 具有明确例外的明确规则

❌ **“考虑...”** - 克劳德应该总是这样做还是只是有时这样做？
✅ **“如果 X，则 Y”** 或 **“总是...”** - 明确的条件
</ambiguous_phrases>

<example>
❌ **不明确**：
```xml
<validation>
You should probably validate the output and try to fix any errors.
</validation>
```


✅ **清除**：
```xml
<validation>
Always validate output before proceeding:

```bash

蟒蛇scripts/validate.py输出目录/
```

If validation fails, fix errors and re-validate. Only proceed when validation passes with zero errors.
</validation>
```

</example>
</avoid_ambiguity>

<define_edge_cases>
<principle>
预测边缘情况并定义如何处理它们。别让克劳德胡乱猜测。
</principle>

<without_edge_cases>
```xml
<quick_start>
Extract email addresses from the text file and save to a JSON array.
</quick_start>
```


**未解答的问题**：
- 如果没有找到电子邮件怎么办？
- 如果同一封电子邮件出现多次怎么办？
- 如果电子邮件格式错误怎么办？
- JSON 到底是什么格式？
</without_edge_cases>

<with_edge_cases>
```xml
<quick_start>
Extract email addresses from the text file and save to a JSON array.

<edge_cases>
- **No emails found**: Save empty array `[]`
- **Duplicate emails**: Keep only unique emails
- **Malformed emails**: Skip invalid formats, log to stderr
- **Output format**: Array of strings, one email per element
</edge_cases>

<example_output>
```json

[
  “user1@example.com”，
  “user2@example.com”
]
```
</example_output>
</quick_start>
```

</with_edge_cases>
</define_edge_cases>

<output_format_specification>
<principle>
当输出格式很重要时，请精确指定。显示示例。
</principle>

<vague_format>
```xml
<output>
Generate a report with the analysis results.
</output>
```

</vague_format>

<specific_format>
```xml
<output_format>
Generate a markdown report with this exact structure:

```markdown

# 分析报告：[标题]

## 执行摘要
[1-2 段总结主要发现]

## 主要发现
- 找到 1 并提供支持数据
- 发现 2 并提供支持数据
- 发现 3 并提供支持数据

## 建议
1. 具体可行的建议
2. 具体可行的建议

## 附录
【原始数据和详细计算】
```

**Requirements**:
- Use exactly these section headings
- Executive summary must be 1-2 paragraphs
- List 3-5 key findings
- Provide 2-4 recommendations
- Include appendix with source data
</output_format>
```

</specific_format>
</output_format_specification>

<decision_criteria>
<principle>
当克劳德必须做出决定时，提供明确的标准。
</principle>

<no_criteria>
```xml
<workflow>
Analyze the data and decide which visualization to use.
</workflow>
```


**问题**：什么因素应该指导这个决定？
</no_criteria>

<with_criteria>
```xml
<workflow>
Analyze the data and select appropriate visualization:

<decision_criteria>
**Use bar chart when**:
- Comparing quantities across categories
- Fewer than 10 categories
- Exact values matter

**Use line chart when**:
- Showing trends over time
- Continuous data
- Pattern recognition matters more than exact values

**Use scatter plot when**:
- Showing relationship between two variables
- Looking for correlations
- Individual data points matter
</decision_criteria>
</workflow>
```


**好处**：克劳德有客观的标准来做决定而不是猜测。
</with_criteria>
</decision_criteria>

<constraints_and_requirements>
<principle>
明确区分“必须做”、“最好有”和“不能做”。
</principle>

<unclear_requirements>
```xml
<requirements>
The report should include financial data, customer metrics, and market analysis. It would be good to have visualizations. Don't make it too long.
</requirements>
```


**问题**：
- 三种内容类型都需要吗？
- 可视化是可选的还是必需的？
- “太长”有多长？
</unclear_requirements>

<clear_requirements>
```xml
<requirements>
<must_have>
- Financial data (revenue, costs, profit margins)
- Customer metrics (acquisition, retention, lifetime value)
- Market analysis (competition, trends, opportunities)
- Maximum 5 pages
</must_have>

<nice_to_have>
- Charts and visualizations
- Industry benchmarks
- Future projections
</nice_to_have>

<must_not>
- Include confidential customer names
- Exceed 5 pages
- Use technical jargon without definitions
</must_not>
</requirements>
```


**好处**：明确的优先事项和限制可以防止不一致。
</clear_requirements>
</constraints_and_requirements>

<success_criteria>
<principle>
定义成功是什么样子。克劳德怎么知道它成功了？
</principle>

<without_success_criteria>
```xml
<objective>
Process the CSV file and generate a report.
</objective>
```


**问题**：这个任务什么时候完成？什么定义了成功？
</without_success_criteria>

<with_success_criteria>
```xml
<objective>
Process the CSV file and generate a summary report.
</objective>

<success_criteria>
- All rows in CSV successfully parsed
- No data validation errors
- Report generated with all required sections
- Report saved to output/report.md
- Output file is valid markdown
- Process completes without errors
</success_criteria>
```


**好处**：明确的完成标准消除了任务何时完成的歧义。
</with_success_criteria>
</success_criteria>

<testing_clarity>
<principle>
通过询问以下问题来测试您的指令：“我可以将这些指令交给初级开发人员并期望得到正确的结果吗？”
</principle>

<testing_process>
1.阅读你的技能说明
2.删除只有你拥有的背景（项目知识、未陈述的假设）
3. 识别不明确的术语或模糊的要求
4. 根据需要添加特异性
5. 与不了解你的背景的人进行测试
6. 根据他们的问题和困惑进行迭代

如果一个背景极少的人陷入困境，克劳德也会如此。
</testing_process>
</testing_clarity>

<practical_examples>
<example domain="data_processing">
❌ **不清楚**：
```xml
<quick_start>
Clean the data and remove bad entries.
</quick_start>
```


✅ **清除**：
```xml
<quick_start>
<data_cleaning>
1. Remove rows where required fields (name, email, date) are empty
2. Standardize date format to YYYY-MM-DD
3. Remove duplicate entries based on email address
4. Validate email format (must contain @ and domain)
5. Save cleaned data to output/cleaned_data.csv
</data_cleaning>

<success_criteria>
- No empty required fields
- All dates in YYYY-MM-DD format
- No duplicate emails
- All emails valid format
- Output file created successfully
</success_criteria>
</quick_start>
```

</example>

<example domain="code_generation">
❌ **不清楚**：
```xml
<quick_start>
Write a function to process user input.
</quick_start>
```


✅ **清除**：
```xml
<quick_start>
<function_specification>
Write a Python function with this signature:

```python

def process_user_input(raw_input: str) -> 字典:
    ”“”
    验证并解析用户输入。

参数：
        raw_input：来自用户的原始字符串（格式：“姓名：电子邮件：年龄”）

返回：
        带键的字典：姓名 (str)、电子邮件 (str)、年龄 (int)

加薪：
        ValueError：如果输入格式无效
    ”“”
```

**Requirements**:
- Split input on colon delimiter
- Validate email contains @ and domain
- Convert age to integer, raise ValueError if not numeric
- Return dictionary with specified keys
- Include docstring and type hints
</function_specification>

<success_criteria>
- Function signature matches specification
- All validation checks implemented
- Proper error handling for invalid input
- Type hints included
- Docstring included
</success_criteria>
</quick_start>
```

</example>
</practical_examples>