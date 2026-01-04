---
name: resolve_pr_parallel
description: 使用并行处理解决所有 PR 评论

argument-hint: "[optional: PR number or current PR]"
---
使用并行处理解决所有 PR 评论。

Claude Code 自动检测并理解您的 git 上下文：

- 当前分支检测
- 相关的公关背景
- 所有公关评论和评论主题
- 可以通过指定 PR 编号或询问来使用任何 PR。

## 工作流程

### 1. 分析

获取所有未解决的 PR 评论

```bash
gh pr status
bin/get-pr-comments PR_NUMBER
```


### 2. 计划

创建按类型分组的所有未解决项目的 TodoWrite 列表。

### 3. 实施（并行）

为每个未解决的项目并行生成一个 pr-comment-resolver 代理。

因此，如果有 3 个评论，它将并行生成 3 个 pr-comment-resolver 代理。列克这个

1.任务pr-comment-resolver(comment1)
2.任务pr-comment-resolver(comment2)
3.任务pr-comment-resolver(comment3)

始终为每个待办事项项目并行运行所有子代理/任务。

### 4. 提交并解决

- 提交更改
- 运行 bin/resolve-pr-thread THREAD_ID_1
- 推送到远程

最后，再次检查 bin/get-pr-comments PR_NUMBER 以查看是否所有评论均已解决。如果没有，他们应该重复从 1 开始的过程。