---
name: playwright-test
description: 在受当前 PR 或分支影响的页面上运行 Playwright 浏览器测试

argument-hint: "[PR number, branch name, or 'current' for current branch]"
---
# 剧作家测试命令

<command_purpose>使用 Playwright MCP 在受 PR 或分支更改影响的页面上运行端到端浏览器测试。</command_purpose>

## 介绍

<role>QA 工程师，专门从事基于浏览器的端到端测试</role>

此命令在真实浏览器中测试受影响的页面，捕获单元测试遗漏的问题：
- JavaScript 集成错误
- CSS/布局回归
- 用户工作流程中断
- 控制台错误

## 先决条件

<requirements>
- 运行本地开发服务器（例如，`bin/dev`、`rails server`）
- 连接剧作家 MCP 服务器
- Git 存储库，包含要测试的更改
</requirements>

## 主要任务

### 1. 确定测试范围

<test_target>$参数</test_target>

<determine_scope>

**如果提供 PR 号码：**
```bash
gh pr view [number] --json files -q '.files[].path'
```


**如果“当前”或为空：**
```bash
git diff --name-only main...HEAD
```


**如果提供分行名称：**
```bash
git diff --name-only main...[branch]
```


</determine_scope>

### 2. 将文件映射到路由

<file_to_route_mapping>

将更改的文件映射到可测试的路径：

|文件模式|路线 |
|----------|----------|
| `app/views/users/*` | `/users`、`/users/:id`、`/users/new` |
| `app/controllers/settings_controller.rb` | `/settings` |
| `app/javascript/controllers/*_controller.js` |使用 Stimulus 控制器的页面 |
| `app/components/*_component.rb` |渲染该组件的页面 |
| `app/views/layouts/*` |所有页面（至少测试主页）|
| `app/assets/stylesheets/*` |关键页面上的视觉回归 |
| `app/helpers/*_helper.rb` |使用该帮助程序的页面|

根据映射构建要测试的 URL 列表。

</file_to_route_mapping>

### 3. 验证服务器是否正在运行

<check_server>

测试之前，请验证本地服务器是否可访问：

```
mcp__playwright__browser_navigate({ url: "http://localhost:3000" })
mcp__playwright__browser_snapshot({})
```


如果服务器未运行，通知用户：
```markdown
**Server not running**

Please start your development server:
- Rails: `bin/dev` or `rails server`
- Node: `npm run dev`

Then run `/playwright-test` again.
```


</check_server>

### 4. 测试每个受影响的页面

<test_pages>

对于每条受影响的路线：

**第 1 步：导航并捕获快照**
```
mcp__playwright__browser_navigate({ url: "http://localhost:3000/[route]" })
mcp__playwright__browser_snapshot({})
```


**步骤 2：检查错误**
```
mcp__playwright__browser_console_messages({ level: "error" })
```


**第 3 步：验证关键要素**
- 页面标题/标题存在
- 呈现的主要内容
- 没有可见的错误消息
- 表单有预期字段

**步骤 4：测试关键交互（如果适用）**
```
mcp__playwright__browser_click({ element: "[description]", ref: "[ref]" })
mcp__playwright__browser_snapshot({})
```


</test_pages>

### 5. 人工验证（需要时）

<human_verification>

测试触摸时暂停以等待人工输入：

|流量类型|问什么 |
|------------|-------------|
| OAuth | “请使用 [provider] 登录并确认其有效” |
|电子邮件 | “检查您的收件箱中是否有测试电子邮件并确认收到”|
|付款 | “在沙盒模式下完成测试购买”|
|短信| “验证您收到短信代码”|
|外部 API | “确认[服务]集成正在运行”|

使用询问用户问题：
```markdown
**Human Verification Needed**

This test touches the [flow type]. Please:
1. [Action to take]
2. [What to verify]

Did it work correctly?
1. Yes - continue testing
2. No - describe the issue
```


</human_verification>

### 6. 处理失败

<failure_handling>

当测试失败时：

1. **记录失败：**
   - 错误状态截图
   - 捕获控制台错误
   - 注意准确的复制步骤

2. **询问用户如何继续：**
   ```markdown
   **Test Failed: [route]**

   Issue: [description]
   Console errors: [if any]

   How to proceed?
   1. Fix now - I'll help debug and fix
   2. Create todo - Add to todos/ for later
   3. Skip - Continue testing other pages
   ```


3. **如果“立即修复”：**
   - 调查问题
   - 提出修复建议
   - 应用修复
   - 重新运行失败的测试

4. **如果“创建待办事项”：**
   - 创建`{id}-pending-p1-playwright-{description}.md`
   - 继续测试

5. **如果“跳过”：**
   - 记录为已跳过
   - 继续测试

</failure_handling>

### 7. 测试总结

<test_summary>

所有测试完成后，呈现总结：

```markdown
## 🎭 Playwright Test Results

**Test Scope:** PR #[number] / [branch name]
**Server:** http://localhost:3000

### Pages Tested: [count]

| Route | Status | Notes |
|-------|--------|-------|
| `/users` | ✅ Pass | |
| `/settings` | ✅ Pass | |
| `/dashboard` | ❌ Fail | Console error: [msg] |
| `/checkout` | ⏭️ Skip | Requires payment credentials |

### Console Errors: [count]
- [List any errors found]

### Human Verifications: [count]
- OAuth flow: ✅ Confirmed
- Email delivery: ✅ Confirmed

### Failures: [count]
- `/dashboard` - [issue description]

### Created Todos: [count]
- `005-pending-p1-playwright-dashboard-error.md`

### Result: [PASS / FAIL / PARTIAL]
```


</test_summary>

## 快速使用示例

```bash
# Test current branch changes
/playwright-test

# Test specific PR
/playwright-test 847

# Test specific branch
/playwright-test feature/new-dashboard
```

