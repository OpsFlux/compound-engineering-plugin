<overview>
本参考涵盖了技能创作中复杂工作流程、验证循环和反馈循环的模式。所有模式都使用纯 XML 结构。
</overview>

<complex_workflows>
<principle>
将复杂的操作分解为清晰、连续的步骤。对于特别复杂的工作流程，请提供清单。
</principle>

<pdf_forms_example>
```xml
<objective>
Fill PDF forms with validated data from JSON field mappings.
</objective>

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

</pdf_forms_example>

<when_to_use>
在以下情况下使用清单模式：
- 工作流程有 5 个以上的连续步骤
- 步骤必须按顺序完成
- 进度跟踪有助于防止错误
- 中断后轻松恢复很有价值
</when_to_use>
</complex_workflows>

<feedback_loops>
<validate_fix_repeat_pattern>
<principle>
运行验证器→修复错误→重复。这种模式极大地提高了输出质量。
</principle>

<document_editing_example>
```xml
<objective>
Edit OOXML documents with XML validation at each step.
</objective>

<editing_process>
<step_1>
Make your edits to `word/document.xml`
</step_1>

<step_2>
**Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
</step_2>

<step_3>
If validation fails:
- Review the error message carefully
- Fix the issues in the XML
- Run validation again
</step_3>

<step_4>
**Only proceed when validation passes**
</step_4>

<step_5>
Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
</step_5>

<step_6>
Test the output document
</step_6>
</editing_process>

<validation>
Never skip validation. Catching errors early prevents corrupted output files.
</validation>
```

</document_editing_example>

<why_it_works>
- 在应用更改之前尽早捕获错误
- 可通过客观验证进行机器验证
- 计划可以迭代而不触及原始计划
- 减少总迭代周期
</why_it_works>
</validate_fix_repeat_pattern>

<plan_validate_execute_pattern>
<principle>
当克劳德执行复杂的、开放式的任务时，以结构化格式创建计划，验证它，然后执行。

工作流程：分析 → **创建计划文件** → **验证计划** → 执行 → 验证
</principle>

<batch_update_example>
```xml
<objective>
Apply batch updates to spreadsheet with plan validation.
</objective>

<workflow>
<plan_phase>
<step_1>
Analyze the spreadsheet and requirements
</step_1>

<step_2>
Create `changes.json` with all planned updates
</step_2>
</plan_phase>

<validation_phase>
<step_3>
Validate the plan: `python scripts/validate_changes.py changes.json`
</step_3>

<step_4>
If validation fails:
- Review error messages
- Fix issues in changes.json
- Validate again
</step_4>

<step_5>
Only proceed when validation passes
</step_5>
</validation_phase>

<execution_phase>
<step_6>
Apply changes: `python scripts/apply_changes.py changes.json`
</step_6>

<step_7>
Verify output
</step_7>
</execution_phase>
</workflow>

<success_criteria>
- Plan validation passes with zero errors
- All changes applied successfully
- Output verification confirms expected results
</success_criteria>
```

</batch_update_example>

<implementation_tip>
使验证脚本变得详细并包含特定的错误消息：

**好的错误消息**：
“未找到字段‘signature_date’。可用字段：customer_name、order_total、signature_date_signed”

**错误的错误消息**：
“无效字段”

特定错误可帮助 Claude 无需猜测即可解决问题。
</implementation_tip>

<when_to_use>
在以下情况下使用计划-验证-执行：
- 操作复杂且容易出错
- 更改是不可逆转或难以撤消的
- 规划可以独立验证
- 尽早发现错误可以节省大量时间
</when_to_use>
</plan_validate_execute_pattern>
</feedback_loops>

<conditional_workflows>
<principle>
通过清晰的分支逻辑引导克劳德完成决策点。
</principle>

<document_modification_example>
```xml
<objective>
Modify DOCX files using appropriate method based on task type.
</objective>

<workflow>
<decision_point_1>
Determine the modification type:

**Creating new content?** → Follow "Creation workflow"
**Editing existing content?** → Follow "Editing workflow"
</decision_point_1>

<creation_workflow>
<objective>Build documents from scratch</objective>

<steps>
1. Use docx-js library
2. Build document from scratch
3. Export to .docx format
</steps>
</creation_workflow>

<editing_workflow>
<objective>Modify existing documents</objective>

<steps>
1. Unpack existing document
2. Modify XML directly
3. Validate after each change
4. Repack when complete
</steps>
</editing_workflow>
</workflow>

<success_criteria>
- Correct workflow chosen based on task type
- All steps in chosen workflow completed
- Output file validated and verified
</success_criteria>
```

</document_modification_example>

<when_to_use>
在以下情况下使用条件工作流程：
- 不同的任务类型需要不同的方法
- 决策点清晰且定义明确
- 工作流程是相互排斥的
- 引导克劳德走上正确的道路可以改善结果
</when_to_use>
</conditional_workflows>

<validation_scripts>
<principles>
验证脚本是力量倍增器。他们发现克劳德可能错过的错误，并提供可操作的反馈来解决问题。
</principles>

<characteristics_of_good_validation>
<verbose_errors>
**好**：“未找到字段‘signature_date’。可用字段：customer_name、order_total、signature_date_signed”

**坏**：“无效字段”

详细错误帮助 Claude 在一次迭代中解决问题，而不是进行多轮猜测。
</verbose_errors>

<specific_feedback>
**好**：“第 47 行：预期结束标记`</paragraph>`，但发现`</section>`”

**坏**：“XML 语法错误”

具体的反馈可以查明问题的确切位置和性质。
</specific_feedback>

<actionable_suggestions>
**好**：“缺少必填字段‘customer_name’。添加：{\"customer_name\": \"value\"}"

**坏**：“缺少必填字段”

可行的建议向克劳德明确指出了需要解决的问题。
</actionable_suggestions>

<available_options>
当验证失败时，显示可用的有效选项：

**好**：“无效状态‘pending_review’。有效状态：活动、暂停、存档”

**坏**：“状态无效”

显示有效选项可以消除猜测。
</available_options>
</characteristics_of_good_validation>

<implementation_pattern>
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
- **Invalid value**: "Invalid status 'pending_review'. Valid statuses: active, paused, archived"

Only proceed when validation passes with zero errors.
</validation>
```

</implementation_pattern>

<benefits>
- 在错误传播之前捕获错误
- 减少迭代周期
- 提供学习反馈
- 使调试具有确定性
- 实现自信的执行
</benefits>
</validation_scripts>

<iterative_refinement>
<principle>
许多工作流程受益于迭代：生成→验证→细化→验证→最终确定。
</principle>

<implementation_example>
```xml
<objective>
Generate reports with iterative quality improvement.
</objective>

<workflow>
<iteration_1>
**Generate initial draft**

Create report based on data and requirements.
</iteration_1>

<iteration_2>
**Validate draft**

Run: `python scripts/validate_report.py draft.md`

Fix any structural issues, missing sections, or data errors.
</iteration_2>

<iteration_3>
**Refine content**

Improve clarity, add supporting data, enhance visualizations.
</iteration_3>

<iteration_4>
**Final validation**

Run: `python scripts/validate_report.py final.md`

Ensure all quality criteria met.
</iteration_4>

<iteration_5>
**Finalize**

Export to final format and deliver.
</iteration_5>
</workflow>

<success_criteria>
- Final validation passes with zero errors
- All quality criteria met
- Report ready for delivery
</success_criteria>
```

</implementation_example>

<when_to_use>
在以下情况下使用迭代求精：
- 多次通过可提高质量
- 验证提供可操作的反馈
- 时间允许迭代
- 完美的输出比速度更重要
</when_to_use>
</iterative_refinement>

<checkpoint_pattern>
<principle>
对于较长的工作流程，请添加检查点，克劳德可以在继续之前暂停并验证进度。
</principle>

<implementation_example>
```xml
<workflow>
<phase_1>
**Data collection** (Steps 1-3)

1. Extract data from source
2. Transform to target format
3. **CHECKPOINT**: Verify data completeness

Only continue if checkpoint passes.
</phase_1>

<phase_2>
**Data processing** (Steps 4-6)

4. Apply business rules
5. Validate transformations
6. **CHECKPOINT**: Verify processing accuracy

Only continue if checkpoint passes.
</phase_2>

<phase_3>
**Output generation** (Steps 7-9)

7. Generate output files
8. Validate output format
9. **CHECKPOINT**: Verify final output

Proceed to delivery only if checkpoint passes.
</phase_3>
</workflow>

<checkpoint_validation>
At each checkpoint:
1. Run validation script
2. Review output for correctness
3. Verify no errors or warnings
4. Only proceed when validation passes
</checkpoint_validation>
```

</implementation_example>

<benefits>
- 防止级联错误
- 更容易诊断问题
- 清晰的进度指标
- 复习时的自然停顿点
- 减少早期错误造成的浪费工作
</benefits>
</checkpoint_pattern>

<error_recovery>
<principle>
设计具有清晰错误恢复路径的工作流程。克劳德应该知道出现问题时该怎么做。
</principle>

<implementation_example>
```xml
<workflow>
<normal_path>
1. Process input file
2. Validate output
3. Save results
</normal_path>

<error_recovery>
**If validation fails in step 2:**
- Review validation errors
- Check if input file is corrupted → Return to step 1 with different input
- Check if processing logic failed → Fix logic, return to step 1
- Check if output format wrong → Fix format, return to step 2

**If save fails in step 3:**
- Check disk space
- Check file permissions
- Check file path validity
- Retry save with corrected conditions
</error_recovery>

<escalation>
**If error persists after 3 attempts:**
- Document the error with full context
- Save partial results if available
- Report issue to user with diagnostic information
</escalation>
</workflow>
```

</implementation_example>

<when_to_use>
在以下情况下包括错误恢复：
- 工作流程与外部系统交互
- 文件操作可能会失败
- 网络调用可能会超时
- 用户输入可能无效
- 错误是可恢复的
</when_to_use>
</error_recovery>