---
name: resolve_todo_parallel
description: 使用并行处理解决所有待处理的 CLI 待办事项

argument-hint: "[optional: specific todo ID or pattern]"
---
使用并行处理解决所有 TODO 注释。

## 工作流程

### 1. 分析

从 /todos/\*.md 目录中获取所有未解决的 TODO

### 2. 计划

创建按类型分组的所有未解决项目的 TodoWrite 列表。确保查看可能发生的依赖关系并优先考虑其他人需要的依赖关系。例如，如果您需要更改名称，则必须等待才能更改其他名称。输出一个美人鱼流程图，展示我们如何做到这一点。我们可以并行地做所有事情吗？我们是否需要先做一个事情，然后再并行进行其他事情？我将按流程将待办事项放入美人鱼图中，以便代理知道如何按顺序进行。

### 3. 实施（并行）

为每个未解决的项目并行生成一个 pr-comment-resolver 代理。

因此，如果有 3 个评论，它将并行生成 3 个 pr-comment-resolver 代理。列克这个

1.任务pr-comment-resolver(comment1)
2.任务pr-comment-resolver(comment2)
3.任务pr-comment-resolver(comment3)

始终为每个待办事项项目并行运行所有子代理/任务。

### 4. 提交并解决

- 提交更改
- 从文件中删除 TODO，并将其标记为已解决。
- 推送到远程