<overview>
通过迭代和测试来提高技能。该参考资料涵盖了评估驱动的开发、Claude A/B 测试模式以及测试期间的 XML 结构验证。
</overview>

<evaluation_driven_development>
<principle>
在编写大量文档之前创建评估。这可以确保您的技能能够解决实际问题，而不是记录想象的问题。
</principle>

<workflow>
<step_1>
**找出差距**：让克劳德在没有技能的情况下执行代表性任务。记录具体的失败或缺失的上下文。
</step_1>

<step_2>
**创建评估**：构建三个场景来测试这些差距。
</step_2>

<step_3>
**建立基线**：在没有技能的情况下衡量克劳德的表现。
</step_3>

<step_4>
**编写最少的说明**：创建足够的内容来弥补差距并通过评估。
</step_4>

<step_5>
**迭代**：执行评估、与基线进行比较并进行改进。
</step_5>
</workflow>

<evaluation_structure>
```json
{
  "skills": ["pdf-processing"],
  "query": "Extract all text from this PDF file and save it to output.txt",
  "files": ["test-files/document.pdf"],
  "expected_behavior": [
    "Successfully reads the PDF file using appropriate library",
    "Extracts text content from all pages without missing any",
    "Saves extracted text to output.txt in clear, readable format"
  ]
}
```

</evaluation_structure>

<why_evaluations_first>
- 阻止记录想象的问题
- 迫使人们清楚地了解成功是什么样子
- 提供技能有效性的客观衡量
- 使技能专注于实际需求
- 启用定量改进跟踪
</why_evaluations_first>
</evaluation_driven_development>

<iterative_development_with_claude>
<principle>
最有效的技能发展利用克劳德本身。与“Claude A”（帮助完善的专家）合作，创建“Claude B”（执行任务的代理）使用的技能。
</principle>

<creating_skills>
<workflow>
<step_1>
**无需技巧即可完成任务**：与 Claude A 一起解决问题，注意您反复提供的上下文。
</step_1>

<step_2>
**要求 Claude A 创建技能**：“创建一个捕捉我们刚刚使用的模式的技能”
</step_2>

<step_3>
**简洁性审查**：删除不必要的解释。
</step_3>

<step_4>
**改进架构**：通过渐进式披露来组织内容。
</step_4>

<step_5>
**与 Claude B 一起测试**：使用新实例来测试真实任务。
</step_5>

<step_6>
**基于观察进行迭代**：返回 Claude A 并观察到具体问题。
</step_6>
</workflow>

<insight>
克劳德模型本身就理解技能格式。只需要求 Claude 创建一项技能，它就会生成结构正确的 SKILL.md 内容。
</insight>
</creating_skills>

<improving_skills>
<workflow>
<step_1>
**在实际工作流程中使用技能**：给 Claude B 分配实际任务。
</step_1>

<step_2>
**观察行为**：它在哪里挣扎、成功或做出意想不到的选择？
</step_2>

<step_3>
**返回 Claude A**：分享观察结果和当前的 SKILL.md。
</step_3>

<step_4>
**审核建议**：Claude A 可能会建议重组、更强的语言或工作流程重组。
</step_4>

<step_5>
**应用并测试**：更新技能并再次测试。
</step_5>

<step_6>
**重复**：根据实际使用情况而不是假设继续。
</step_6>
</workflow>

<what_to_watch_for>
- **意外的探索路径**：结构可能不直观
- **错过连接**：链接可能需要更明确
- **过度依赖部分**：考虑将经常阅读的内容移至主 SKILL.md
- **忽略的内容**：信号不佳或不必要的文件
- **关键元数据**：技能元数据中的名称和描述对于发现至关重要
</what_to_watch_for>
</improving_skills>
</iterative_development_with_claude>

<model_testing>
<principle>
使用您计划使用的所有模型进行测试。不同的模型有不同的优势，需要不同程度的细节。
</principle>

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

<balancing_across_models>
适用于 Opus 的内容可能需要更多适用于 Haiku 的细节。旨在获得适用于所有目标模型的说明。找到为目标受众服务的平衡点。

有关模型测试示例，请参阅 [core-principles.md](core-principles.md)。
</balancing_across_models>
</model_testing>

<xml_structure_validation>
<principle>
在测试期间，验证您的技能的 XML 结构是否正确且完整。
</principle>

<validation_checklist>
更新技能后，验证：

<required_tags_present>
- ✅`<objective>`标签存在并定义了技能的作用
- ✅ `<quick_start>` 标签存在并提供即时指导
- ✅ `<success_criteria>` 或 `<when_successful>` 标签存在
</required_tags_present>

<no_markdown_headings>
- ✅ 技能正文中没有 `#`、`##` 或 `###` 标题
- ✅ 所有部分都使用 XML 标签
- ✅ 保留标签内的 Markdown 格式（粗体、斜体、列表、代码块）
</no_markdown_headings>

<proper_xml_nesting>
- ✅ 所有 XML 标签都正确关闭
- ✅ 嵌套标签具有正确的层次结构
- ✅ 没有未关闭的标签
</proper_xml_nesting>

<conditional_tags_appropriate>
- ✅ 条件标签匹配技能复杂性
- ✅ 简单的技能仅使用所需的标签
- ✅复杂技能添加适当的条件标签
- ✅ 没有过度设计或规格不足
</conditional_tags_appropriate>

<reference_files_check>
- ✅ 参考文件也使用纯XML结构
- ✅ 参考文件的链接正确
- ✅ 参考资料来自 SKILL.md 深一级
</reference_files_check>
</validation_checklist>

<testing_xml_during_iteration>
当迭代技能时：

1. 更改XML结构
2. **验证XML结构**（检查标签、嵌套、完整性）
3. 与 Claude 一起测试代表性任务
4. 观察 XML 结构是否有助于或阻碍 Claude 的理解
5. 根据实际性能迭代结构
</testing_xml_during_iteration>
</xml_structure_validation>

<observation_based_iteration>
<principle>
根据您观察到的情况而不是您的假设进行迭代。实际使用揭示了假设遗漏的问题。
</principle>

<observation_categories>
<what_claude_reads>
克劳德实际上读了哪些部分？哪些被忽略了？这揭示了：
- 内容的相关性
- 渐进式披露的有效性
- 章节名称是否清晰
</what_claude_reads>

<where_claude_struggles>
哪些任务会导致混乱或错误？这揭示了：
- 缺少上下文
- 指示不明确
- 例子不足
- 要求不明确
</where_claude_struggles>

<where_claude_succeeds>
哪些任务进展顺利？这揭示了：
- 有效的模式
- 好例子
- 清晰的指示
- 适当的细节水平
</where_claude_succeeds>

<unexpected_behaviors>
克劳德做了什么让你惊讶的事？这揭示了：
- 未说明的假设
- 措辞不明确
- 缺少约束
- 其他解释
</unexpected_behaviors>
</observation_categories>

<iteration_pattern>
1. **观察**：让克劳德以当前技能执行实际任务
2. **文件**：注意具体问题，而不是笼统感受
3. **假设**：为什么会出现这个问题？
4. **修复**：进行有针对性的更改以解决特定问题
5. **测试**：验证修复是否适用于相同场景
6. **验证**：确保修复不会破坏其他场景
7. **重复**：继续处理下一个观察到的问题
</iteration_pattern>
</observation_based_iteration>

<progressive_refinement>
<principle>
技能最初不需要是完美的。从最少的开始，观察使用情况，添加缺少的内容。
</principle>

<initial_version>
从以下开始：
- 有效的 YAML frontmatter
- 必需的 XML 标签：objective、quick_start、success_criteria
- 最小工作示例
- 基本成功标准

最初跳过：
- 广泛的例子
- 边缘案例文档
- 高级功能
- 详细的参考文件
</initial_version>

<iteration_additions>
通过迭代添加：
- 描述中模式不清楚的示例
- 实际使用中观察到的边缘情况
- 用户需要时的高级功能
- SKILL.md接近500行时的参考文件
- 常见错误时的验证脚本
</iteration_additions>

<benefits>
- 更快地达到初始工作版本
- 添加解决实际需求，而不是想象的需求
- 保持技能集中且简洁
- 渐进式披露自然而然地出现
- 文档与实际使用保持一致
</benefits>
</progressive_refinement>

<testing_discovery>
<principle>
测试克劳德能否在适当的时候发现并使用你的技能。
</principle>

<discovery_testing>
<test_description>
测试克劳德是否在应该加载你的技能时：

1. 开始新的对话（Claude B）
2.提出应该触发技能的问题
3.检查技能是否加载
4. 验证技能是否正确使用
</test_description>

<description_quality>
如果没有发现技能：
- 检查描述是否包含触发关键字
- 验证描述是否具体，不模糊
- 确保描述解释了何时使用技能
- 使用同一请求的不同措辞进行测试

描述是克劳德的主要发现机制。
</description_quality>
</discovery_testing>
</testing_discovery>

<common_iteration_patterns>
<pattern name="too_verbose">
**观察**：技能有效，但使用大量代币

**修复**：
- 删除明显的解释
- 假设克劳德知道常见概念
- 使用示例而不是冗长的描述
- 将高级内容移至参考文件
</pattern>

<pattern name="too_minimal">
**观察**：克劳德做出了错误的假设或错过了步骤

**修复**：
- 在假设失败的地方添加明确的说明
- 提供完整的工作示例
- 定义边缘情况
- 添加验证步骤
</pattern>

<pattern name="poor_discovery">
**观察**：技能存在，但克劳德在需要时没有加载它

**修复**：
- 改进特定触发器的描述
- 添加相关关键词
- 针对实际用户查询测试描述
- 使有关用例的描述更加具体
</pattern>

<pattern name="unclear_structure">
**观察**：克劳德读错了部分或错过了相关内容

**修复**：
- 使用更清晰的 XML 标签名称
- 重新组织内容层次结构
- 提前移动经常需要的内容
- 添加相关部分的明确链接
</pattern>

<pattern name="incomplete_examples">
**观察**：克劳德产生的输出与预期模式不匹配

**修复**：
- 添加更多显示模式的示例
- 使示例更加完整
- 在示例中显示边缘情况
- 添加反模式示例（不该做什么）
</pattern>
</common_iteration_patterns>

<iteration_velocity>
<principle>
小而频繁的迭代胜过大规模、不频繁的重写。
</principle>

<fast_iteration>
**好方法**：
1. 做出一项有针对性的改变
2.具体场景测试
3.验证改进
4. 提交变更
5. 转到下一期

总时间：每次迭代分钟数
每天迭代：10-20
学习率：高
</fast_iteration>

<slow_iteration>
**有问题的方法**：
1. 积累很多问题
2. 进行大规模重构
3.一次性测试所有内容
4.同时调试多个问题
5. 很难知道什么修复了什么

总时间：每次迭代的小时数
每天迭代次数：1-2
学习率：低
</slow_iteration>

<benefits_of_fast_iteration>
- 隔离因果关系
- 更快地构建模式识别
- 减少错误方向造成的浪费工作
- 如果需要更容易恢复
- 保持势头
</benefits_of_fast_iteration>
</iteration_velocity>

<success_metrics>
<principle>
定义如何衡量该技能是否有效。量化成功。
</principle>

<objective_metrics>
- **成功率**：正确完成任务的百分比
- **令牌使用**：每个任务消耗的平均令牌
- **迭代计数**：尝试获得正确输出的次数
- **错误率**：有错误的任务百分比
- **发现率**：技能加载的频率
</objective_metrics>

<subjective_metrics>
- **输出质量**：输出是否满足要求？
- **适当的细节**：太冗长或太少？
- **克劳德信心**：克劳德看起来不确定吗？
- **用户满意度**：技能能否解决实际问题？
</subjective_metrics>

<tracking_improvement>
比较更改前后的指标：
- 基线：无需技巧即可测量
- 初始：使用第一个版本进行测量
- 迭代 N：每次更改后进行测量

跟踪哪些更改改善了哪些指标。加倍关注有效的模式。
</tracking_improvement>
</success_metrics>