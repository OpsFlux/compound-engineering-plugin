---
name: report-bug
description: 报告复合工程插件中的错误

argument-hint: "[optional: brief description of the bug]"
---
# 报告复合工程插件错误

报告使用复合工程插件时遇到的错误。此命令收集结构化信息并为维护者创建 GitHub 问题。

## 第 1 步：收集错误信息

使用 AskUserQuestion 工具收集以下信息：

**问题 1：Bug 类别**
- 您遇到什么类型的问题？
- 选项：代理不工作、命令不工作、技能不工作、MCP 服务器问题、安装问题、其他

**问题2：特定组件**
- 哪个特定组件受到影响？
- 询问代理、命令、技能或 MCP 服务器的名称

**问题 3：发生了什么（实际行为）**
- 问：“当你使用这个组件时发生了什么？”
- 获得实际行为的清晰描述

**问题 4：应该发生什么（预期行为）**
- 问：“你预计会发生什么？”
- 获得预期行为的清晰描述

**问题 5：重现步骤**
- 问：“在错误发生之前您采取了哪些步骤？”
- 获取复制步骤

**问题 6：错误消息**
- 询问：“您看到任何错误消息吗？如果有，请分享。”
- 捕获任何错误输出

## 步骤2：收集环境信息

自动采集：
```bash
# Get plugin version
cat ~/.claude/plugins/installed_plugins.json 2>/dev/null | grep -A5 "compound-engineering" | head -10 || echo "Plugin info not found"

# Get Claude Code version
claude --version 2>/dev/null || echo "Claude CLI version unknown"

# Get OS info
uname -a
```


## 步骤 3：格式化错误报告

使用以下内容创建结构良好的错误报告：

```markdown
## Bug Description

**Component:** [Type] - [Name]
**Summary:** [Brief description from argument or collected info]

## Environment

- **Plugin Version:** [from installed_plugins.json]
- **Claude Code Version:** [from claude --version]
- **OS:** [from uname]

## What Happened

[Actual behavior description]

## Expected Behavior

[Expected behavior description]

## Steps to Reproduce

1. [Step 1]
2. [Step 2]
3. [Step 3]

## Error Messages

```

[任何错误输出]
```

## Additional Context

[Any other relevant information]

---
*Reported via `/report-bug` command*
```


## 步骤 4：创建 GitHub 问题

使用 GitHub CLI 创建问题：

```bash
gh issue create \
  --repo EveryInc/every-marketplace \
  --title "[compound-engineering] Bug: [Brief description]" \
  --body "[Formatted bug report from Step 3]" \
  --label "bug,compound-engineering"
```


**注意：** 如果标签不存在，则创建无标签：
```bash
gh issue create \
  --repo EveryInc/every-marketplace \
  --title "[compound-engineering] Bug: [Brief description]" \
  --body "[Formatted bug report]"
```


## 步骤5：确认提交

创建问题后：
1.向用户显示问题URL
2. 感谢他们报告错误
3. 让他们知道维护者 (Kieran Klaassen) 将收到通知

## 输出格式

```
✅ Bug report submitted successfully!

Issue: https://github.com/EveryInc/every-marketplace/issues/[NUMBER]
Title: [compound-engineering] Bug: [description]

Thank you for helping improve the compound-engineering plugin!
The maintainer will review your report and respond as soon as possible.
```


## 错误处理

- 如果`gh` CLI未经过身份验证：提示用户首先运行`gh auth login`
- 如果问题创建失败：显示格式化报告，以便用户可以手动创建问题
- 如果缺少所需信息：重新提示该特定字段

## 隐私声明

此命令不收集：
- 个人信息
- API 密钥或凭证
- 您项目中的私有代码
- 超出基本操作系统信息的文件路径

报告中仅包含有关该错误的技术信息。