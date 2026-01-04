---
name: file-todos
description: 在管理 todos/ 目录中基于文件的待办事项跟踪系统时，应该使用此技能。它提供了用于创建待办事项、管理状态和依赖项、进行分类以及与斜杠命令和代码审查流程集成的工作流程。

---
# 基于文件的 Todo 跟踪技能

## 概述

`todos/` 目录包含一个基于文件的跟踪系统，用于管理代码审查反馈、技术债务、功能请求和工作项目。每个待办事项都是一个带有 YAML frontmatter 和结构化部分的 markdown 文件。

该技能应该在以下情况下使用：
- 根据发现或反馈创建新的待办事项
- 管理待办事项生命周期（待处理 → 准备就绪 → 完成）
- 对待批准的项目进行分类
- 检查或管理依赖关系
- 将 PR 评论或代码发现转化为跟踪工作
- 在待办事项执行期间更新工作日志

## 文件命名约定

Todo 文件遵循以下命名模式：

```
{issue_id}-{status}-{priority}-{description}.md
```


**组件：**
- **issue_id**：序列号（001、002、003...） - 从未重复使用
- **状态**：`pending`（需要分类）、`ready`（已批准）、`complete`（已完成）
- **优先级**：`p1`（关键）、`p2`（重要）、`p3`（必备）
- **描述**：kebab-case，简要描述

**示例：**
```
001-pending-p1-mailer-test.md
002-ready-p1-fix-n-plus-1.md
005-complete-p2-refactor-csv.md
```


## 文件结构

每个待办事项都是一个带有 YAML frontmatter 和结构化部分的 markdown 文件。创建新待办事项时，请使用 [todo-template.md](./assets/todo-template.md) 中的模板作为起点。

**必填部分：**
- **问题陈述** - 有什么问题、缺失或需要改进？
- **调查结果** - 调查结果、根本原因、关键发现
- **建议的解决方案** - 具有优点/缺点、努力、风险的多种选项
- **建议采取的行动** - 清晰的计划（在分类期间填写）
- **验收标准** - 可测试清单项目
- **工作日志** - 按时间顺序记录日期、行动、学习情况

**可选部分：**
- **技术细节** - 受影响的文件、相关组件、数据库更改
- **资源** - 错误、测试、PR、文档的链接
- **注释** - 其他背景或决定

**YAML frontmatter 字段：**
```yaml
---
status: ready              # pending | ready | complete
priority: p1              # p1 | p2 | p3
issue_id: "002"
tags: [rails, performance, database]
dependencies: ["001"]     # Issue IDs this is blocked by
---
```


## 常见工作流程

### 创建一个新的待办事项

**根据发现或反馈创建新的待办事项：**

1. 确定下一期 ID：`ls todos/ | grep -o '^[0-9]\+' | sort -n | tail -1`
2.复制模板：`cp assets/todo-template.md todos/{NEXT_ID}-pending-{priority}-{description}.md`
3. 编辑并填写所需部分：
   - 问题陈述
   - 调查结果（如果来自调查）
   - 建议的解决方案（多个选项）
   - 验收标准
   - 添加初始工作日志条目
4. 确定状态：`pending`（需要分类）或`ready`（预先批准）
5.添加相关标签进行过滤

**何时创建待办事项：**
- 需要超过15-20分钟的工作时间
- 需要研究、规划或考虑多种方法
- 依赖于其他工作
- 需要经理批准或确定优先级
- 较大功能或重构的一部分
- 技术债务需要文件

**何时立即采取行动：**
- 问题很简单（< 15 分钟）
- 完整的上下文现已可用
- 无需规划
- 用户明确请求立即采取行动
- 简单的错误修复和明显的解决方案

### 对待处理项目进行分类

**对待处理的待办事项进行分类：**

1. 列出待处理项目：`ls todos/*-pending-*.md`
2. 对于每个待办事项：
   - 阅读问题陈述和调查结果
   - 审查建议的解决方案
   - 做出决定：批准、推迟或修改优先级
3. 更新已批准的待办事项：
   - 重命名文件：`mv {file}-pending-{pri}-{desc}.md {file}-ready-{pri}-{desc}.md`
   - 更新前言：`status: pending` → `status: ready`
   - 在“建议行动”部分填写清晰的计划
   - 如果与初始评估不同，则调整优先级
4. 延期待办事项保持 `pending` 状态

**使用斜线命令：** `/triage` 进行交互式审批工作流程

### 管理依赖关系

**跟踪依赖关系：**

```yaml
dependencies: ["002", "005"]  # This todo blocked by issues 002 and 005
dependencies: []               # No blockers - can work immediately
```


**要检查是什么阻止了待办事项：**
```bash
grep "^dependencies:" todos/003-*.md
```


**要查找待办事项阻止的内容：**
```bash
grep -l 'dependencies:.*"002"' todos/*.md
```


**开始前验证拦截器是否完整：**
```bash
for dep in 001 002 003; do
  [ -f "todos/${dep}-complete-*.md" ] || echo "Issue $dep not complete"
done
```


### 更新工作日志

**处理待办事项时，始终添加工作日志条目：**

```markdown
### YYYY-MM-DD - Session Title

**By:** Claude Code / Developer Name

**Actions:**
- Specific changes made (include file:line references)
- Commands executed
- Tests run
- Results of investigation

**Learnings:**
- What worked / what didn't
- Patterns discovered
- Key insights for future work
```


工作日志的作用是：
- 调查历史记录
- 尝试的方法的记录
- 团队知识共享
- 未来类似工作的背景

### 完成待办事项

**将待办事项标记为完成：**

1. 核对所有验收标准
2. 使用最终会议和结果更新工作日志
3. 重命名文件：`mv {file}-ready-{pri}-{desc}.md {file}-complete-{pri}-{desc}.md`
4. 更新标题：`status: ready` → `status: complete`
5. 检查是否畅通工作：`grep -l 'dependencies:.*"002"' todos/*-ready-*.md`
6. 提交问题参考：`feat: resolve issue 002`

## 与开发工作流程集成

|触发|流量|工具|
|---------|------|------|
|代码审查 | `/workflows:review` → 调查结果 → `/triage` → 待办事项 |审核代理+技能|
|公关评论 | `/resolve_pr_parallel` → 个别修复 → Todos | gh CLI + 技能 |
|代码 TODO | `/resolve_todo_parallel` → 修复 + 复杂的待办事项 |代理+技能|
|规划|集思广益 → 创建待办事项 → 工作 → 完成 |技能|
|反馈 |讨论 → 创建待办事项 → 分类 → 工作 |技能+斩击|

## 快速参考命令

**找工作：**
```bash
# List highest priority unblocked work
grep -l 'dependencies: \[\]' todos/*-ready-p1-*.md

# List all pending items needing triage
ls todos/*-pending-*.md

# Find next issue ID
ls todos/ | grep -o '^[0-9]\+' | sort -n | tail -1 | awk '{printf "%03d", $1+1}'

# Count by status
for status in pending ready complete; do
  echo "$status: $(ls -1 todos/*-$status-*.md 2>/dev/null | wc -l)"
done
```


**依赖管理：**
```bash
# What blocks this todo?
grep "^dependencies:" todos/003-*.md

# What does this todo block?
grep -l 'dependencies:.*"002"' todos/*.md
```


**搜索中：**
```bash
# Search by tag
grep -l "tags:.*rails" todos/*.md

# Search by priority
ls todos/*-p1-*.md

# Full-text search
grep -r "payment" todos/
```


## 主要区别

**文件待办事项系统（此技能）：**
- `todos/`目录中的Markdown文件
- 开发/项目跟踪
- 带有 YAML frontmatter 的独立 markdown 文件
- 由人类和代理使用

**Rails Todo 模型：**
- `app/models/todo.rb`中的数据库模型
- 应用程序中面向用户的功能
- Active Record CRUD操作
- 与这种基于文件的系统不同

**TodoWrite 工具：**
- 代理会话期间的内存中任务跟踪
- 单个对话的临时跟踪
- 未持久化到磁盘
- 与上述两个系统不同