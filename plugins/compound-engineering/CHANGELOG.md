# 变更日志

复合工程插件的所有显着更改都将记录在此文件中。

该格式基于[保留变更日志](https://keepachangelog.com/en/1.0.0/)，
并且该项目遵循[语义版本控制](https://semver.org/spec/v2.0.0.html)。

## [2.20.0] - 2026-01-01

### 已更改

- **`create-agent-skills`技能** - 完全重写以符合Anthropic的官方技能规范：
  - **格式更改**：技能现在使用标准 Markdown 标题（`## Quick Start`、`## Instructions`），而不是 XML 标签。以前的版本错误地推荐了 XML 标签，这不是官方格式。
  - **命名约定**：根据官方规范更新为使用动名词形式（`creating-agent-skills`、`processing-pdfs`）
  - **描述格式**：必须是第三人称，包括使用内容和时间
  - 添加了`references/official-spec.md` - 来自 code.claude.com/docs/en/skills 的 Anthropic 官方技能规范
  - 添加了`references/best-practices.md` - 来自 platform.claude.com 的技能创作最佳实践
  - 删除了过时的 `references/use-xml-tags.md` - 这是不正确的指导

### 哲学

此更新使该技能与 Anthropic 的官方文档保持一致。关键见解：**技能就是提示**。所有标准提示最佳实践均适用。使用标准 Markdown，而不是自定义 XML 标签。将 SKILL.md 控制在 500 行以内，并逐步公开参考文件。

---

## [2.19.0] - 2025-12-31

### 添加

- **`/deepen-plan`命令** - 计划的力量增强。采用现有计划并为每个主要部分运行并行研究子代理以添加：
  - 最佳实践和行业模式
  - 性能优化
  - UI/UX 改进（如果适用）
  - 质量增强和边缘情况
  - 现实世界的实施示例

其结果是一个根深蒂固、可投入生产的计划以及具体的实施细节。

### 已更改

- **`/workflows:plan` 命令** - 添加`/deepen-plan` 作为生成后菜单中的选项 2。添加注释：如果在启用 ultrathink 的情况下运行，则会自动运行 Deep-plan 以获得最大深度。

## [2.18.0] - 2025-12-25

### 添加

- **`agent-native-architecture`技能** - 添加了**动态能力发现**模式和**架构审查清单**：

**mcp-tool-design.md 中的新模式：**
  - **动态功能发现** - 对于外部 API（HealthKit、HomeKit、GraphQL），构建一个在运行时返回可用功能的发现工具（`list_*`），以及一个接受字符串（非枚举）的通用访问工具。验证的是 API，而不是您的代码。这意味着代理可以使用新的 API 功能，而无需更改代码。
  - **CRUD 完整性** - 代理可以创建的每个实体也必须是可读、可更新和可删除的。不完整的 CRUD = 操作奇偶校验被破坏。

**SKILL.md 中的新功能：**
  - **架构审查清单** - 将审查者的发现尽早推入设计阶段。涵盖工具设计（动态与静态、CRUD 完整性）、操作奇偶性（功能图、编辑/删除）、UI 集成（代理 → UI 通信）和上下文注入。
  - **选项 11：API 集成** - 用于连接到 HealthKit、HomeKit、GraphQL 等外部 API 的新摄入选项
  - **新的反模式：** 静态工具映射（为每个 API 端点构建单独的工具）、不完整的 CRUD（仅创建工具）
  - **工具设计标准**部分添加到成功标准清单中

**shared-workspace-architecture.md 中的新功能：**
  - **用于多设备同步的 iCloud 文件存储** - 使用 iCloud 文档作为共享工作区，无需构建同步层即可免费自动进行多设备同步。包括实现模式、冲突处理、权利以及何时不使用它。

### 哲学

此更新为**代理本机应用程序**编写了一个关键见解：在与代理应具有与用户相同的访问权限的外部 API 集成时，使用**动态功能发现**而不是静态工具映射。不是构建 `read_steps`、`read_heart_rate`、`read_sleep`...，而是构建 `list_health_types` + `read_health_data(dataType: string)`。代理发现可用的内容，API 验证类型。

注意：此模式专门适用于遵循“用户能做什么，代理也能做什么”理念的代理本机应用程序。对于具有故意限制能力的受限代理，静态工具映射可能是合适的。

---

## [2.17.0] - 2025-12-25

### 增强

- **`agent-native-architecture` 技能** - 基于构建 Every Reader iOS 应用程序的现实学习经验的主要扩展。添加了 5 个新的参考文档并扩展了现有的参考文档：

**新参考文献：**
  - **dynamic-context-injection.md** - 如何将运行时应用程序状态注入代理系统提示中。涵盖上下文注入模式、注入什么上下文（资源、活动、功能、词汇）、Swift/iOS 和 TypeScript 的实现模式以及上下文新鲜度。
  - **action-parity-discipline.md** - 确保代理可以执行用户可以执行的所有操作的工作流程。包括能力映射模板、奇偶校验流程、PR 检查表、奇偶校验工具设计和上下文奇偶校验指南。
  - **shared-workspace-architecture.md** - 在同一数据空间中工作的代理和用户的模式。涵盖目录结构、文件工具、UI 集成（文件监视、共享存储）、代理-用户协作模式和安全注意事项。
  - **agent-native-testing.md** - 代理本机应用程序的测试模式。包括“代理能做到吗？”测试、惊喜测试、自动奇偶校验测试、集成测试和 CI/CD 集成。
  - **mobile-patterns.md** - iOS/Android 的移动特定模式。涵盖后台执行（检查点/恢复）、权限处理、成本感知设计（模型层、代币预算、网络感知）、离线处理和电池感知。

**更新参考资料：**
  - **architecture-patterns.md** - 添加了 3 个新模式：统一代理架构（一个协调器，多种代理类型）、代理到 UI 通信（共享数据存储、文件监视、事件总线）和模型层选择（快速/平衡/强大）。

**更新的技能根：**
  - **SKILL.md** - 扩展了入口菜单（现在有 10 个选项，包括上下文注入、操作奇偶校验、共享工作区、测试、移动模式）。添加了 5 个新的代理本机反模式（上下文匮乏、孤立功能、沙箱隔离、静默操作、能力隐藏）。通过代理本机和移动特定的清单扩展了成功标准。

- **`agent-native-reviewer` 代理** - 通过涵盖所有新模式的全面审查流程显着增强。现在检查操作奇偶性、上下文奇偶性、共享工作区、工具设计（基元与工作流）、动态上下文注入和特定于移动设备的问题。包括详细的反模式、输出格式模板、快速检查（“写入位置”测试、惊喜测试）和特定于移动设备的验证。

### 哲学

这些更新将构建代理本机移动应用程序的关键见解付诸实践：**“代理应该能够通过镜像 UI 功能的工具以及有关应用程序状态的完整上下文来执行用户可以执行的任何操作。”** 促使这些更改的失败案例：代理询问“什么阅读提要？”当用户说“在我的阅读提要中写一些东西”时，因为它没有 `publish_to_feed` 工具，也没有关于“提要”含义的上下文。

## [2.16.0] - 2025-12-21

### 增强

- **`dhh-rails-style` 技能** - 大量扩展的参考文档，纳入了 Marc Köhlbrugge 的非官方 37signals 编码风格指南中的模式：
  - **controllers.md** - 添加了授权模式、速率限制、Sec-Fetch-Site CSRF 保护、请求上下文问题
  - **models.md** - 添加了验证哲学、让它崩溃哲学（bang 方法）、lambda 的默认值、Rails 7.1+ 模式（规范化、委托类型、存储访问器）、触摸链的关注准则
  - **frontend.md** - 添加了 Turbo 变形最佳实践、Turbo 帧模式、6 个新的 Stimulus 控制器（自动提交、对话框、本地时间等）、Stimulus 最佳实践、视图助手、个性化缓存、广播模式
  - **architecture.md** - 添加了基于路径的多租户、数据库模式（UUID、状态记录、硬删除、计数器缓存）、后台作业模式（事务安全、错误处理、批处理）、电子邮件模式、安全模式（XSS、SSRF、CSP）、活动存储模式
  - **gems.md** - 添加了扩展的“他们要避免什么”部分（服务对象、表单对象、装饰器、CSS 预处理器、React/Vue）、使用 Minitest/fixtures 模式的测试理念

- **`dhh-ruby-style`技能** - 扩展patterns.md：
  - 开发理念（发布/验证/完善，解决根本原因，香草Rails优先）
  - Rails 7.1+ 习语（params.expect、StringInquirer、肯定的命名约定）
  - 提取指南（三规则，复杂时从控制器提取开始）

### 制作人员

- 参考模式源自 [Marc Köhlbrugge 的非官方 37signals 编码风格指南](https://github.com/marckohlbrugge/unofficial-37signals-coding-style-guide)

## [2.15.2] - 2025-12-21

### 固定的

- **所有技能** - 修复了 13 项技能的规范合规性问题：
  - 参考文件现在使用正确的降价链接（`[file.md](./references/file.md)`）而不是反引号文本
  - 根据技能创建者规范，描述现在使用第三人称（“该技能应该在……时使用”）
  - 受影响的技能：agent-native-architecture、andrew-kane-gem-writer、compound-docs、create-agent-skills、dhh-rails-style、dhh-ruby-style、dspy-ruby、every-style-editor、file-todos、frontend-design、gemini-imagegen

### 添加

- **CLAUDE.md** - 添加了带有验证命令的技能合规性检查表，以确保新技能满足规范要求

## [2.15.1] - 2025-12-18

### 已更改

- **`/workflows:review` 命令** - 第 7 节现在可以检测项目类型（Web、iOS 或混合）并提供适当的测试。 Web 项目获得`/playwright-test`，iOS 项目获得`/xcode-test`，混合项目可以运行两者。

## [2.15.0] - 2025-12-18

### 添加

- **`/xcode-test` 命令** - 使用 XcodeBuildMCP 在模拟器上构建和测试 iOS 应用程序。自动检测 Xcode 项目、构建应用程序、启动模拟器并运行测试套件。包括对片状测试的重试。

- **`/playwright-test` 命令** - 在受当前 PR 或分支影响的页面上运行 Playwright 浏览器测试。检测更改的文件，映射到受影响的路线，生成/运行有针对性的测试，并通过屏幕截图报告结果。