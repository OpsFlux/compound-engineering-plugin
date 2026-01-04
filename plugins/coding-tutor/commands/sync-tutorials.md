# 同步编码导师教程

提交您的教程并将其推送到 GitHub 存储库以供备份和移动阅读。

## 指示

1. **转到教程存储库**：`cd ~/coding-tutor-tutorials`

2. **检查更改**：运行`git status`以查看新增内容或修改内容

3. **如果有变化**：
   - 上演所有变更：`git add -A`
   - 创建一个提交，其中包含总结添加/更新内容的消息（例如，“添加 React hooks 教程”或“更新测验分数”）
   - 推至原点：`git push`

4. **如果不存在 GitHub 远程**：
   - 创建存储库：`gh repo create coding-tutor-tutorials --private --source=. --push`

5. **报告结果**：告诉用户已同步的内容或所有内容都已是最新的

## 注释

- 教程存储库位于：`~/coding-tutor-tutorials/`
- 创建 GitHub 存储库时始终使用 `--private`
- 这是您的个人学习之旅 - 做好备份！