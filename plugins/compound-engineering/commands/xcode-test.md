---
name: xcode-test
description: 使用 XcodeBuildMCP 在模拟器上构建和测试 iOS 应用程序

argument-hint: "[scheme name or 'current' to use default]"
---
# Xcode 测试命令

<command_purpose>使用 XcodeBuildMCP 在模拟器上构建、安装和测试 iOS 应用程序。捕获屏幕截图、日志并验证应用程序行为。</command_purpose>

## 介绍

<role>iOS QA 工程师，专门从事基于模拟器的测试</role>

此命令通过以下方式测试 iOS/macOS 应用程序：
- 模拟器构建
- 安装并启动应用程序
- 截取关键屏幕的屏幕截图
- 捕获控制台日志以查找错误
- 支持外部流程人工验证

## 先决条件

<requirements>
- 使用命令行工具安装 Xcode
- XcodeBuildMCP 服务器已连接
- 有效的 Xcode 项目或工作区
- 至少一个可用的 iOS 模拟器
</requirements>

## 主要任务

### 0.验证 XcodeBuildMCP 是否已安装

<check_mcp_installed>

**首先，检查 XcodeBuildMCP 工具是否可用。**

尝试致电：
```
mcp__xcodebuildmcp__list_simulators({})
```


**如果该工具未找到或出现错误：**

告诉用户：
```markdown
**XcodeBuildMCP not installed**

Please install the XcodeBuildMCP server first:

\`\`\`bash
claude mcp add XcodeBuildMCP -- npx xcodebuildmcp@latest
\`\`\`

Then restart Claude Code and run `/xcode-test` again.
```


**在确认 XcodeBuildMCP 正常工作之前，请勿继续**。

</check_mcp_installed>

### 1. 发现项目和方案

<discover_project>

**查找可用项目：**
```
mcp__xcodebuildmcp__discover_projs({})
```


**列出该项目的方案：**
```
mcp__xcodebuildmcp__list_schemes({ project_path: "/path/to/Project.xcodeproj" })
```


**如果提供参数：**
- 使用指定的方案名称
- 或“当前”使用默认/上次使用的方案

</discover_project>

### 2.启动模拟器

<boot_simulator>

**列出可用的模拟器：**
```
mcp__xcodebuildmcp__list_simulators({})
```


**启动首选模拟器（推荐iPhone 15 Pro）：**
```
mcp__xcodebuildmcp__boot_simulator({ simulator_id: "[uuid]" })
```


**等待模拟器准备就绪：**
在继续安装之前检查模拟器状态。

</boot_simulator>

### 3. 构建应用程序

<build_app>

**为 iOS 模拟器构建：**
```
mcp__xcodebuildmcp__build_ios_sim_app({
  project_path: "/path/to/Project.xcodeproj",
  scheme: "[scheme_name]"
})
```


**处理构建失败：**
- 捕获构建错误
- 为每个构建错误创建 P1 待办事项
- 向用户报告具体错误详细信息

**成功时：**
- 记下构建的应用程序安装路径
- 继续安装步骤

</build_app>

### 4. 安装并启动

<install_launch>

**在模拟器上安装应用程序：**
```
mcp__xcodebuildmcp__install_app_on_simulator({
  app_path: "/path/to/built/App.app",
  simulator_id: "[uuid]"
})
```


**启动应用程序：**
```
mcp__xcodebuildmcp__launch_app_on_simulator({
  bundle_id: "[app.bundle.id]",
  simulator_id: "[uuid]"
})
```


**开始捕获日志：**
```
mcp__xcodebuildmcp__capture_sim_logs({
  simulator_id: "[uuid]",
  bundle_id: "[app.bundle.id]"
})
```


</install_launch>

### 5. 测试关键屏幕

<test_screens>

对于应用程序中的每个关键屏幕：

**截图：**
```
mcp__xcodebuildmcp__take_screenshot({
  simulator_id: "[uuid]",
  filename: "screen-[name].png"
})
```


**查看屏幕截图：**
- UI 元素正确呈现
- 没有可见的错误消息
- 预期显示的内容
- 布局看起来正确

**检查日志是否有错误：**
```
mcp__xcodebuildmcp__get_sim_logs({ simulator_id: "[uuid]" })
```


寻找：
- 崩溃
- 例外情况
- 错误级别日志消息
- 网络请求失败

</test_screens>

### 6. 人工验证（需要时）

<human_verification>

测试触摸时暂停以等待人工输入：

|流量类型|问什么 |
|------------|-------------|
|使用 Apple 登录 | “请在模拟器上完成使用 Apple 登录”|
|推送通知 | “发送测试推送并确认其出现” |
|应用内购买 | “完成沙盒购买”|
|相机/照片| “授予权限并验证相机是否正常工作” |
|地点 | “允许位置访问并验证地图更新” |

使用询问用户问题：
```markdown
**Human Verification Needed**

This test requires [flow type]. Please:
1. [Action to take on simulator]
2. [What to verify]

Did it work correctly?
1. Yes - continue testing
2. No - describe the issue
```


</human_verification>

### 7. 处理失败

<failure_handling>

当测试失败时：

1. **记录失败：**
   - 截取错误状态的屏幕截图
   - 捕获控制台日志
   - 注意复制步骤

2. **询问用户如何继续：**
   ```markdown
   **Test Failed: [screen/feature]**

   Issue: [description]
   Logs: [relevant error messages]

   How to proceed?
   1. Fix now - I'll help debug and fix
   2. Create todo - Add to todos/ for later
   3. Skip - Continue testing other screens
   ```


3. **如果“立即修复”：**
   - 调查代码中的问题
   - 提出修复建议
   - 重建并重新测试

4. **如果“创建待办事项”：**
   - 创建`{id}-pending-p1-xcode-{description}.md`
   - 继续测试

</failure_handling>

### 8. 测试总结

<test_summary>

所有测试完成后，呈现总结：

```markdown
## 📱 Xcode Test Results

**Project:** [project name]
**Scheme:** [scheme name]
**Simulator:** [simulator name]

### Build: ✅ Success / ❌ Failed

### Screens Tested: [count]

| Screen | Status | Notes |
|--------|--------|-------|
| Launch | ✅ Pass | |
| Home | ✅ Pass | |
| Settings | ❌ Fail | Crash on tap |
| Profile | ⏭️ Skip | Requires login |

### Console Errors: [count]
- [List any errors found]

### Human Verifications: [count]
- Sign in with Apple: ✅ Confirmed
- Push notifications: ✅ Confirmed

### Failures: [count]
- Settings screen - crash on navigation

### Created Todos: [count]
- `006-pending-p1-xcode-settings-crash.md`

### Result: [PASS / FAIL / PARTIAL]
```


</test_summary>

### 9. 清理

<cleanup>

测试后：

**停止日志捕获：**
```
mcp__xcodebuildmcp__stop_log_capture({ simulator_id: "[uuid]" })
```


**可选择关闭模拟器：**
```
mcp__xcodebuildmcp__shutdown_simulator({ simulator_id: "[uuid]" })
```


</cleanup>

## 快速使用示例

```bash
# Test with default scheme
/xcode-test

# Test specific scheme
/xcode-test MyApp-Debug

# Test after making changes
/xcode-test current
```


## 与 /workflows:review 集成

当审查涉及 iOS 代码的 PR 时，`/workflows:review` 命令可以将其生成为子代理：

```
Task general-purpose("Run /xcode-test for scheme [name]. Build, install on simulator, test key screens, check for crashes.")
```

