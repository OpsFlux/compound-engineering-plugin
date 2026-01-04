---
name: heal-skill
description: 当技能有错误说明或过时的 API 参考时修复不正确的 SKILL.md 文件

argument-hint: [optional: specific issue to fix]
allowed-tools: [Read, Edit, Bash(ls:*), Bash(git:*)]
---
<objective>
根据执行过程中发现的更正更新技能的 SKILL.md 和相关文件。

分析对话以检测正在运行的技能，反思出现的问题，提出具体修复方案，获得用户批准，然后通过可选提交应用更改。
</objective>

<context>
技能检测：！`ls -1 ./skills/*/SKILL.md | head -5`
</context>

<quick_start>
<workflow>
1. **从对话上下文中检测技能**（调用消息、最近的 SKILL.md 参考文献）
2. **反思**出了什么问题以及您如何发现解决方案
3. **呈现**建议的更改以及之前/之后的差异
4. **进行任何编辑之前获得批准**
5. **应用**更改并可选择提交
</workflow>
</quick_start>

<process>
<step_1 name="detect_skill">
从对话上下文中识别技能：

- 寻找技能调用消息
- 检查最近引用了哪个 SKILL.md
- 检查当前任务上下文

套装：`SKILL_NAME=[skill-name]` 和 `SKILL_DIR=./skills/$SKILL_NAME`

如果不清楚，请询问用户。
</step_1>

<step_2 name="reflection_and_analysis">
重点关注 $ARGUMENTS（如果提供），否则分析更广泛的上下文。

确定：
- **出了什么问题**：引用 SKILL.md 中不正确的特定部分
- **发现方法**：Context7、错误消息、尝试和错误、文档查找
- **根本原因**：过时的 API、不正确的参数、错误的端点、缺少上下文
- **影响范围**：单部分还是多部分？相关文件受影响吗？
- **建议的修复**：哪些文件、哪些部分、每个文件之前/之后
</step_2>

<step_3 name="scan_affected_files">
```bash
ls -la $SKILL_DIR/
ls -la $SKILL_DIR/references/ 2>/dev/null
ls -la $SKILL_DIR/scripts/ 2>/dev/null
```

</step_3>

<step_4 name="present_proposed_changes">
以这种格式呈现更改：

```
**Skill being healed:** [skill-name]
**Issue discovered:** [1-2 sentence summary]
**Root cause:** [brief explanation]

**Files to be modified:**
- [ ] SKILL.md
- [ ] references/[file].md
- [ ] scripts/[file].py

**Proposed changes:**

### Change 1: SKILL.md - [Section name]
**Location:** Line [X] in SKILL.md

**Current (incorrect):**
```

[当前文件中的确切文本]
```

**Corrected:**
```

[新文本]
```

**Reason:** [why this fixes the issue]

[repeat for each change across all files]

**Impact assessment:**
- Affects: [authentication/API endpoints/parameters/examples/etc.]

**Verification:**
These changes will prevent: [specific error that prompted this]
```

</step_4>

<step_5 name="request_approval">
```
Should I apply these changes?

1. Yes, apply and commit all changes
2. Apply but don't commit (let me review first)
3. Revise the changes (I'll provide feedback)
4. Cancel (don't make changes)

Choose (1-4):
```


**等待用户响应。未经批准不得继续。**
</step_5>

<step_6 name="apply_changes">
仅在批准后（选项 1 或 2）：

1. 使用编辑工具对所有文件进行每次更正
2. 读回修改的部分以验证
3. 如果选择 1，请提交结构化消息，显示已修复的内容
4. 通过文件列表确认完成
</step_6>
</process>

<success_criteria>
- 从对话上下文中正确检测到技能
- 所有不正确的部分都标有之前/之后
- 应用前用户批准的更改
- SKILL.md 和相关文件中应用的所有编辑
- 通过回读验证更改
- 如果用户选择选项 1，则创建提交
- 通过文件列表确认完成
</success_criteria>

<verification>
完成之前：

- 读回每个修改的部分以确认已应用的更改
- 确保跨文件一致性（SKILL.md 示例匹配引用/）
- 如果选择了选项 1，则验证已创建的 git 提交
- 检查没有意外的文件被修改
</verification>