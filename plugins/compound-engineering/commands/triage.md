---
name: triage
description: 对 CLI 待办事项系统的结果进行分类和分类

argument-hint: "[findings list or source type]"
---
- 首先将/model设置为俳句
- 然后读取 todos/ 目录中所有待处理的待办事项

在此一一呈现所有发现、决定或问题以进行分类。目标是检查每个项目并决定是否将其添加到 CLI 待办事项系统中。

**重要提示：在分类期间请勿编写任何代码！**

该命令用于：

- 对代码审查结果进行分类
- 处理安全审核结果
- 审查绩效分析
- 处理任何其他需要跟踪的分类结果

## 工作流程

### 第 1 步：展示每项发现

对于每项发现，都以以下格式呈现：

```
---
Issue #X: [Brief Title]

Severity: 🔴 P1 (CRITICAL) / 🟡 P2 (IMPORTANT) / 🔵 P3 (NICE-TO-HAVE)

Category: [Security/Performance/Architecture/Bug/Feature/etc.]

Description:
[Detailed explanation of the issue or improvement]

Location: [file_path:line_number]

Problem Scenario:
[Step by step what's wrong or could happen]

Proposed Solution:
[How to fix it]

Estimated Effort: [Small (< 2 hours) / Medium (2-8 hours) / Large (> 8 hours)]

---
Do you want to add this to the todo list?
1. yes - create todo file
2. next - skip this item
3. custom - modify before creating
```


### 第 2 步：处理用户决策

**当用户说“是”时：**

1. **更新现有待办事项文件**（如果存在）或**创建新文件名：**

如果待办事项已经存在（来自代码审查）：

- 将文件从`{id}-pending-{priority}-{desc}.md`重命名为`{id}-ready-{priority}-{desc}.md`
   - 更新 YAML frontmatter：`status: pending` → `status: ready`
   - 保持issue_id、优先级和描述不变

如果创建新的待办事项：

   ```
   {next_id}-ready-{priority}-{brief-description}.md
   ```


优先级映射：

- 🔴 P1（关键）→ `p1`
   - 🟡 P2（重要）→ `p2`
   - 🔵 P3（必备）→ `p3`

示例：`042-ready-p1-transaction-boundaries.md`

2. **更新 YAML frontmatter：**

   ```yaml
   ---
   status: ready # IMPORTANT: Change from "pending" to "ready"
   priority: p1 # or p2, p3 based on severity
   issue_id: "042"
   tags: [category, relevant-tags]
   dependencies: []
   ---
   ```


3. **填充或更新文件：**

   ```yaml
   # [Issue Title]

   ## Problem Statement
   [Description from finding]

   ## Findings
   - [Key discoveries]
   - Location: [file_path:line_number]
   - [Scenario details]

   ## Proposed Solutions

   ### Option 1: [Primary solution]
   - **Pros**: [Benefits]
   - **Cons**: [Drawbacks if any]
   - **Effort**: [Small/Medium/Large]
   - **Risk**: [Low/Medium/High]

   ## Recommended Action
   [Filled during triage - specific action plan]

   ## Technical Details
   - **Affected Files**: [List files]
   - **Related Components**: [Components affected]
   - **Database Changes**: [Yes/No - describe if yes]

   ## Resources
   - Original finding: [Source of this issue]
   - Related issues: [If any]

   ## Acceptance Criteria
   - [ ] [Specific success criteria]
   - [ ] Tests pass
   - [ ] Code reviewed

   ## Work Log

   ### {date} - Approved for Work
   **By:** Claude Triage System
   **Actions:**
   - Issue approved during triage session
   - Status changed from pending → ready
   - Ready to be picked up and worked on

   **Learnings:**
   - [Context and insights]

   ## Notes
   Source: Triage session on {date}
   ```


4. **确认批准：** “✅ 已批准：`{new_filename}`（问题 #{issue_id}）- 状态：**准备就绪** → 准备工作”

**当用户说“下一步”时：**

- **删除 todo 文件** - 将其从 todos/ 目录中删除，因为它不相关
- 跳到下一个项目
- 跟踪跳过的项目以进行摘要

**当用户说“自定义”时：**

- 询问要修改的内容（优先级、描述、详细信息）
- 更新信息
- 目前修订版本
- 再次询问：是/下一个/自定义

### 第 3 步：继续直至所有内容均已处理完毕

- 一项一项地处理所有项目
- 使用 TodoWrite 进行跟踪以获得可见性
- 不要等待项目之间的批准 - 继续前进

### 步骤 4：最终总结

处理完所有项目后：

````markdown
## Triage Complete

**Total Items:** [X] **Todos Approved (ready):** [Y] **Skipped:** [Z]

### Approved Todos (Ready for Work):

- `042-ready-p1-transaction-boundaries.md` - Transaction boundary issue
- `043-ready-p2-cache-optimization.md` - Cache performance improvement ...

### Skipped Items (Deleted):

- Item #5: [reason] - Removed from todos/
- Item #12: [reason] - Removed from todos/

### Summary of Changes Made:

During triage, the following status updates occurred:

- **Pending → Ready:** Filenames and frontmatter updated to reflect approved status
- **Deleted:** Todo files for skipped findings removed from todos/ directory
- Each approved file now has `status: ready` in YAML frontmatter

### Next Steps:

1. View approved todos ready for work:
   ```bash
   ls todos/*-ready-*.md
   ```
````


2. 开始处理已批准的项目：

   ```bash
   /resolve_todo_parallel  # Work on multiple approved items efficiently
   ```


3. 或者选择要处理的单个项目

4. 在工作时更新待办事项状态：
   - 准备就绪→进行中（在您工作时的当地环境中）
   - 进行中 → 完成（重命名文件：准备就绪 → 完成，更新 frontmatter）

```

## Example Response Format

```


---

问题 #5：缺少多步骤操作的事务边界

严重性：🔴P1（严重）

类别：数据完整性/安全性

说明：GoogleOauthCallbacks 关注点中的 google_oauth2_connected 回调在没有事务保护的情况下执行多个数据库操作。如果任何步骤中途失败，数据库就会处于不一致状态。

地点：app/controllers/concerns/google_oauth_callbacks.rb:13-50

问题场景：

1. User.update成功（邮箱变了）
2. 保存账户！失败（验证错误）
3.结果：用户更改了电子邮件，但没有关联的帐户
4.下次登录尝试完全失败

无交易的操作：

- 用户确认（第 13 行）
- 删除候补名单（第 14 行）
- 用户个人资料更新（第 21-23 行）
- 帐户创建（第 28-37 行）
- 头像附件（第 39-45 行）
- 旅程创建（第 47 行）

建议的解决方案：将所有操作包装在 ApplicationRecord.transaction do ... end 块中

预计工作量：小（30 分钟）

---

您想将其添加到待办事项列表中吗？

1. yes - 创建todo文件
2. 下一步 - 跳过此项
3.自定义-创建前修改

```

## Important Implementation Details

### Status Transitions During Triage

**When "yes" is selected:**
1. Rename file: `{id}-pending-{priority}-{desc}.md` → `{id}-ready-{priority}-{desc}.md`
2. Update YAML frontmatter: `status: pending` → `status: ready`
3. Update Work Log with triage approval entry
4. Confirm: "✅ Approved: `{filename}` (Issue #{issue_id}) - Status: **ready**"

**When "next" is selected:**
1. Delete the todo file from todos/ directory
2. Skip to next item
3. No file remains in the system

### Progress Tracking

Every time you present a todo as a header, include:
- **Progress:** X/Y completed (e.g., "3/10 completed")
- **Estimated time remaining:** Based on how quickly you're progressing
- **Pacing:** Monitor time per finding and adjust estimate accordingly

Example:
```


进度：3/10 完成 |预计时间：剩余约 2 分钟

```

### Do Not Code During Triage

- ✅ Present findings
- ✅ Make yes/next/custom decisions
- ✅ Update todo files (rename, frontmatter, work log)
- ❌ Do NOT implement fixes or write code
- ❌ Do NOT add detailed implementation details
- ❌ That's for /resolve_todo_parallel phase
```


完成后给出这些选项

```markdown
What would you like to do next?

1. run /resolve_todo_parallel to resolve the todos
2. commit the todos
3. nothing, go chill
```

