---
name: workflows:work
description: 高效执行工作计划，同时保持质量和精加工功能

argument-hint: "[plan file, specification, or todo file path]"
---
# 工作计划执行命令

高效执行工作计划，同时保持质量和精加工功能。

## 介绍

该命令获取工作文档（计划、规范或待办事项文件）并系统地执行它。重点是通过快速了解需求、遵循现有模式并始终保持质量来**交付完整的功能**。

## 输入文档

<input_document> #$参数</input_document>

## 执行工作流程

### 第 1 阶段：快速启动

1. **阅读计划并澄清**

- 完整阅读工作文件
   - 查看计划中提供的任何参考资料或链接
   - 如果有任何不清楚或不明确的地方，请立即提出澄清问题
   - 获得用户批准才能继续
   - **不要跳过这个** - 现在提出问题比构建错误的东西更好

2. **设置环境**

选择您的工作方式：

**选项 A：在当前分支上实时工作**
   ```bash
   git checkout main && git pull origin main
   git checkout -b feature-branch-name
   ```


**选项 B：与工作树并行工作（建议并行开发）**
   ```bash
   # Ask user first: "Work in parallel with worktree or on current branch?"
   # If worktree:
   skill: git-worktree
   # The skill will create a new branch from main in an isolated worktree
   ```


**建议**：在以下情况下使用工作树：
   - 您想要同时处理多个功能
   - 您想在实验时保持主体清洁
   - 您打算经常在分行之间切换

如果出现以下情况，请使用实时分支：
   - 您正在开发一项功能
   - 您更喜欢留在主存储库中

3. **创建待办事项列表**
   - 使用 TodoWrite 将计划分解为可操作的任务
   - 包括任务之间的依赖关系
   - 根据需要首先完成的事情确定优先级
   - 包括测试和质量检查任务
   - 保持任务具体且可完成

### 第 2 阶段：执行

1. **任务执行循环**

对于按优先级顺序排列的每个任务：

   ```
   while (tasks remain):
     - Mark task as in_progress in TodoWrite
     - Read any referenced files from the plan
     - Look for similar patterns in codebase
     - Implement following existing conventions
     - Write tests for new functionality
     - Run tests after changes
     - Mark task as completed
   ```


2. **遵循现有模式**

- 该计划应引用类似的代码 - 首先阅读这些文件
   - 完全匹配命名约定
   - 尽可能重用现有组件
   - 遵循项目编码标准（参见 CLAUDE.md）
   - 如有疑问，请 grep 查找类似的实现

3. **持续测试**

- 在每次重大更改后运行相关测试
   - 不要等到结束才进行测试
   - 立即修复故障
   - 添加新功能的新测试

4. **Figma 设计同步**（如果适用）

对于 Figma 设计的 UI 工作：

- 按照设计规范实施组件
   - 迭代使用figma-design-sync代理进行比较
   - 修复发现的视觉差异
   - 重复直到实现与设计匹配

5. **跟踪进度**
   - 完成任务后及时更新 TodoWrite
   - 注意任何阻碍或意外发现
   - 如果范围扩大则创建新任务
   - 让用户了解主要里程碑

### 第 3 阶段：质量检查

1. **运行核心质量检查**

始终在提交之前运行：

   ```bash
   # Run full test suite
   bin/rails test

   # Run linting (per CLAUDE.md)
   # Use linting-agent before pushing to origin
   ```


2. **考虑审稿代理**（可选）

用于复杂、有风险或较大的变更：

- **code-simplicity-reviewer**：检查不必要的复杂性
   - **kieran-rails-reviewer**：验证 Rails 约定（Rails 项目）
   - **性能-oracle**：检查性能问题
   - **安全哨兵**：扫描安全漏洞
   - **cora-test-reviewer**：审查测试质量（CORA 项目）

与任务工具并行运行审阅者：

   ```
   Task(code-simplicity-reviewer): "Review changes for simplicity"
   Task(kieran-rails-reviewer): "Check Rails conventions"
   ```


向用户展示调查结果并解决关键问题。

3. **最终验证**
   - 所有 TodoWrite 任务标记为已完成
   - 所有测试均通过
   - Linting 通行证
   - 代码遵循现有模式
   - Figma 设计匹配（如果适用）
   - 没有控制台错误或警告

### 第 4 阶段：发货

1. **创建提交**

   ```bash
   git add .
   git status  # Review what's being committed
   git diff --staged  # Check the changes

   # Commit with conventional format
   git commit -m "$(cat <<'EOF'
   feat(scope): description of what and why

   Brief explanation if needed.

   🤖 Generated with [Claude Code](https://claude.com/claude-code)

   Co-Authored-By: Claude <noreply@anthropic.com>
   EOF
   )"
   ```


2. **捕获并上传 UI 更改的屏幕截图**（任何 UI 工作都需要）

对于**任何**设计更改、新视图或 UI 修改，您必须捕获并上传屏幕截图：

**第 1 步：启动开发服务器**（如果未运行）
   ```bash
   bin/dev  # Run in background
   ```


**步骤 2：使用 Playwright MCP 工具捕获屏幕截图**
   - `browser_navigate` 前往受影响的页面
   - `browser_resize`设置视口（根据需要在桌面或移动设备上）
   - `browser_snapshot` 验证页面状态
   - `browser_take_screenshot` 捕捉图像

**第3步：使用imgup技能上传**
   ```bash
   skill: imgup
   # Then upload each screenshot:
   imgup -h pixhost screenshot.png  # pixhost works without API key
   # Alternative hosts: catbox, imagebin, beeimg
   ```


**要捕捉什么：**
   - **新屏幕**：新用户界面的屏幕截图
   - **修改的屏幕**：屏幕截图之前和之后
   - **设计实现**：显示 Figma 设计匹配的屏幕截图

**重要**：始终在 PR 描述中包含上传的图像 URL。这为审阅者提供了视觉上下文并记录了更改。

3. **创建拉取请求**

   ```bash
   git push -u origin feature-branch-name

   gh pr create --title "Feature: [Description]" --body "$(cat <<'EOF'
   ## Summary
   - What was built
   - Why it was needed
   - Key decisions made

   ## Testing
   - Tests added/modified
   - Manual testing performed

   ## Before / After Screenshots
   | Before | After |
   |--------|-------|
   | ![before](URL) | ![after](URL) |

   ## Figma Design
   [Link if applicable]

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   EOF
   )"
   ```


4. **通知用户**
   - 总结已完成的工作
   - 公关链接
   - 记下所需的任何后续工作
   - 建议后续步骤（如果适用）

---

## 关键原则

### 快速启动，执行更快

- 一开始就得到澄清，然后执行
- 不要等待完全理解 - 提出问题并行动
- 目标是**完成功能**，而不是创建完美的流程

### 计划是你的指南

- 工作文档应引用类似的代码和模式
- 加载这些参考文献并遵循它们
- 不要重新发明 - 匹配现有的

### 边走边测试

- 在每次更改后运行测试，而不是在最后运行测试
- 立即修复故障
- 持续测试可防止出现重大意外

### 质量是内置的

- 遵循现有模式
- 为新代码编写测试
- 在推送之前运行 linting
- 仅对复杂/有风险的变更使用审阅者代理

### 提供完整的功能

- 在继续之前标记所有已完成的任务
- 不要让功能完成 80%
- 已发布的已完成功能胜过未发布的完美功能

## 质量检查表

在创建 PR 之前，请验证：

- [ ] 所有提出和回答的澄清问题
- [ ] 所有 TodoWrite 任务标记为已完成
- [ ] 测试通过（运行`bin/rails test`）
- [ ] Linting 通行证（使用 linting-agent）
- [ ] 代码遵循现有模式
- [ ] Figma 设计匹配实现（如果适用）
- [ ] 捕获和上传屏幕截图之前/之后（用于 UI 更改）
- [ ] 提交消息遵循常规格式
- [ ] PR 描述包括摘要、测试说明和屏幕截图

## 何时使用审阅代理

**默认情况下不要使用。** 仅在以下情况下使用审阅者代理：

- 影响许多文件的大型重构（10+）
- 安全敏感的更改（身份验证、权限、数据访问）
- 性能关键的代码路径
- 复杂的算法或业务逻辑
- 用户明确要求彻底审查

对于大多数功能：测试 + linting + 以下模式就足够了。

## 要避免的常见陷阱

- **分析瘫痪** - 不要想太多，阅读计划并执行
- **跳过澄清问题** - 现在就问，而不是在构建错误的东西之后
- **忽略计划参考** - 该计划有链接是有原因的
- **最后测试** - 持续测试或稍后受苦
- **忘记 TodoWrite** - 跟踪进度或忘记已完成的操作
- **80% 完成综合症** - 完成功能，不要过早继续
- **过度审查简单的更改** - 节省审阅者代理的复杂工作