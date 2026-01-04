# 推荐的技能结构

复杂技能的最佳结构将路由、工作流程和知识分开。

<structure>
```
skill-name/
├── SKILL.md              # Router + essential principles (unavoidable)
├── workflows/            # Step-by-step procedures (how)
│   ├── workflow-a.md
│   ├── workflow-b.md
│   └── ...
└── references/           # Domain knowledge (what)
    ├── reference-a.md
    ├── reference-b.md
    └── ...
```

</structure>

<why_this_works>
## 解决的问题

**问题 1：上下文被跳过**
当重要的原则放在单独的文件中时，克劳德可能不会阅读它们。
**解决方案：** 将基本原则直接放入 SKILL.md 中。它们会自动加载。

**问题 2：加载了错误的上下文**
“构建”任务加载调试引用。 “调试”任务加载构建引用。
**解决方案：** 接收问题确定意图 → 通往特定工作流程的路线 → 工作流程指定要阅读的参考文献。

**问题 3：单一技能势不可挡**
500 多行混合内容使得很难找到相关部分。
**解决方案：** 小型路由器（SKILL.md）+重点工作流程+参考库。

**问题4：程序与知识混合**
“如何做 X”与“X 的含义”混合在一起会造成混乱。
**解决方案：** 工作流程是过程（步骤）。参考文献是知识（模式、示例）。
</why_this_works>

<skill_md_template>
## 技能.md 模板

```markdown
---
name: skill-name
description: What it does and when to use it.
---

<essential_principles>
## How This Skill Works

[Inline principles that apply to ALL workflows. Cannot be skipped.]

### Principle 1: [Name]
[Brief explanation]

### Principle 2: [Name]
[Brief explanation]
</essential_principles>

<intake>
**Ask the user:**

What would you like to do?
1. [Option A]
2. [Option B]
3. [Option C]
4. Something else

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "keyword", "keyword" | `workflows/option-a.md` |
| 2, "keyword", "keyword" | `workflows/option-b.md` |
| 3, "keyword", "keyword" | `workflows/option-c.md` |
| 4, other | Clarify, then select |

**After reading the workflow, follow it exactly.**
</routing>

<reference_index>
All domain knowledge in `references/`:

**Category A:** file-a.md, file-b.md
**Category B:** file-c.md, file-d.md
</reference_index>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| option-a.md | [What it does] |
| option-b.md | [What it does] |
| option-c.md | [What it does] |
</workflows_index>
```

</skill_md_template>

<workflow_template>
## 工作流程模板

```markdown
# Workflow: [Name]

<required_reading>
**Read these reference files NOW:**
1. references/relevant-file.md
2. references/another-file.md
</required_reading>

<process>
## Step 1: [Name]
[What to do]

## Step 2: [Name]
[What to do]

## Step 3: [Name]
[What to do]
</process>

<success_criteria>
This workflow is complete when:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
</success_criteria>
```

</workflow_template>

<when_to_use_this_pattern>
## 何时使用此模式

**在以下情况下使用路由器+工作流程+参考：**
- 多个不同的工作流程（构建、调试、发布）
- 不同的工作流程需要不同的参考
- 不可忽视的基本原则
- 技能已发展到超过 200 行

**在以下情况下使用简单的单列技能：**
- 一个工作流程
- 小参考集
- 总共不到 200 行
- 没有强制执行的基本原则
</when_to_use_this_pattern>

<key_insight>
## 关键见解

**SKILL.md 始终加载。使用此保证。**

将不可避免的内容放入SKILL.md中：
- 基本原则
- 摄入量问题
- 路由逻辑

将工作流程特定的内容放入工作流程/中：
- 分步程序
- 该工作流程所需的参考资料
- 该工作流程的成功标准

将可重用的知识放在参考文献/中：
- 模式和例子
- 技术细节
- 领域专业知识
</key_insight>