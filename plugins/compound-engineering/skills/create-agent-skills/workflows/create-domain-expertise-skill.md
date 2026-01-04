# 工作流程：创建详尽的领域专业技能

<objective>
培养在特定领域发挥实际作用的全面执行技能。领域专业技能是功能齐全的构建技能，参考中包含详尽的领域知识，整个生命周期的完整工作流程（构建→调试→优化→交付），并且可以由用户直接调用，也可以由其他技能（如创建计划）加载以获取领域知识。
</objective>

<critical_distinction>
**常规技能：**“完成一项特定任务”
**领域专业技能：**“以完整的从业知识做该领域的一切”

示例：
- `expertise/macos-apps` - 从头开始构建 macOS 应用程序
- `expertise/python-games` - 构建具有完整游戏开发生命周期的完整 Python 游戏
- `expertise/rust-systems` - 使用详尽的系统知识构建 Rust 系统程序
- `expertise/web-scraping` - 构建抓取工具，处理所有边缘情况，大规模部署

领域专业技能：
- ✅ 执行任务（构建、调试、优化、交付）
- ✅ 在参考文献中拥有全面的领域知识
- ✅ 由用户直接调用（“构建 macOS 应用程序”）
- ✅ 可以被其他技能加载（create-plan 读取参考进行规划）
- ✅ 涵盖整个生命周期，而不仅仅是开始
</critical_distinction>

<required_reading>
**立即阅读这些参考文件：**
1.references/recommended-structure.md
2.references/core-principles.md
3.references/use-xml-tags.md
</required_reading>

<process>
## 步骤 1：识别域

询问用户要建立哪些领域的专业知识：

**示例域：**
- macOS/iOS应用程序开发
- Python游戏开发
- Rust 系统编程
- 机器学习/人工智能
- 网页抓取和自动化
- 数据工程管道
- 音频处理/DSP
- 3D图形/着色器
- Unity/Unreal游戏开发
- 嵌入式系统

具体一点：“Python 游戏”还是“专门使用 Pygame 的 Python 游戏”？

## 步骤2：确认目标位置

解释一下：
```
Domain expertise skills go in: ~/.claude/skills/expertise/{domain-name}/

These are comprehensive BUILD skills that:
- Execute tasks (build, debug, optimize, ship)
- Contain exhaustive domain knowledge
- Can be invoked directly by users
- Can be loaded by other skills for domain knowledge

Name suggestion: {suggested-name}
Location: ~/.claude/skills/expertise/{suggested-name}/
```


确认或调整名称。

## 步骤 3：确定工作流程

领域专业技能涵盖整个生命周期。确定需要哪些工作流程。

**大多数领域的通用工作流程：**
1. **build-new-{thing}.md** - 从头开始创建
2. **add-feature.md** - 扩展现有的{thing}
3. **debug-{thing}.md** - 查找并修复错误
4. **write-tests.md** - 测试正确性
5. **optimize-performance.md** - 配置文件和加速
6. **ship-{thing}.md** - 部署/分发

**特定领域的工作流程：**
- 游戏：`implement-game-mechanic.md`、`add-audio.md`、`polish-ui.md`
- 网络应用程序：`setup-auth.md`、`add-api-endpoint.md`、`setup-database.md`
- 系统：`optimize-memory.md`、`profile-cpu.md`、`cross-compile.md`

每个工作流程 = 用户实际执行的一种完整任务类型。

## 步骤 4：详尽的研究阶段

**重要：**这项研究必须是全面的，而不是肤浅的。

### 研究策略

运行多次网络搜索以确保覆盖范围：

**搜索 1：当前生态系统**
- “2024 年 2025 年最佳 {domain} 图书馆”
- “流行的{domain}框架比较”
- “{domain} 技术堆栈建议”

**搜索 2：架构模式**
- “{domain}架构模式”
- “{domain} 最佳实践设计模式”
- “如何构建{domain}项目”

**搜索 3：生命周期和工具**
- “{domain} 开发工作流程”
- “{domain} 测试调试最佳实践”
-“{domain}部署分布”

**搜索 4：常见陷阱**
- “{domain} 常见错误要避免”
- “{domain} 反模式”
- “{domain} 不该做什么”

**搜索 5：实际使用情况**
- “{domain} 制作示例 GitHub”
- “{domain}案例研究”
- “成功的{domain}项目”

### 验证要求

对于找到的每个主要库/工具/模式：
- **检查新近度：** 上次更新是什么时候？
- **检查采用情况：** 是否积极维护？社区规模？
- **检查替代方案：** 还有什么存在？何时分别使用？
- **检查弃用：** 是否有任何内容被替换？

**过时内容的危险信号：**
- 2023 年之前的文章（基本概念除外）
- 废弃的库（12 个月以上没有提交）
- 已弃用的 API 或模式
- “这曾经很流行，但是......”

### 文档来源

如果可用，请使用 Context7 MCP：
```
mcp__context7__resolve-library-id: {library-name}
mcp__context7__get-library-docs: {library-id}
```


关注官方文档，而不是教程。

## 步骤 5：将知识组织到各个领域

按领域关注点结构引用，而不是按任意类别。

**对于游戏开发示例：**
```
references/
├── architecture.md         # ECS, component-based, state machines
├── libraries.md           # Pygame, Arcade, Panda3D (when to use each)
├── graphics-rendering.md  # 2D/3D rendering, sprites, shaders
├── physics.md             # Collision, physics engines
├── audio.md               # Sound effects, music, spatial audio
├── input.md               # Keyboard, mouse, gamepad, touch
├── ui-menus.md            # HUD, menus, dialogs
├── game-loop.md           # Update/render loop, fixed timestep
├── state-management.md    # Game states, scene management
├── networking.md          # Multiplayer, client-server, P2P
├── asset-pipeline.md      # Loading, caching, optimization
├── testing-debugging.md   # Unit tests, profiling, debugging tools
├── performance.md         # Optimization, profiling, benchmarking
├── packaging.md           # Building executables, installers
├── distribution.md        # Steam, itch.io, app stores
└── anti-patterns.md       # Common mistakes, what NOT to do
```


**对于 macOS 应用程序开发示例：**
```
references/
├── app-architecture.md     # State management, dependency injection
├── swiftui-patterns.md     # Declarative UI patterns
├── appkit-integration.md   # Using AppKit with SwiftUI
├── concurrency-patterns.md # Async/await, actors, structured concurrency
├── data-persistence.md     # Storage strategies
├── networking.md           # URLSession, async networking
├── system-apis.md          # macOS-specific frameworks
├── testing-tdd.md          # Testing patterns
├── testing-debugging.md    # Debugging tools and techniques
├── performance.md          # Profiling, optimization
├── design-system.md        # Platform conventions
├── macos-polish.md         # Native feel, accessibility
├── security-code-signing.md # Signing, notarization
└── project-scaffolding.md  # CLI-based setup
```


**对于每个参考文件：**
- 纯XML结构
- 决策树：“如果 X，则使用 Y。如果 Z，则使用 A。”
- 比较表：库与库（速度、功能、学习曲线）
- 显示模式的代码示例
- “何时使用”指导
- 特定于平台的注意事项
- 当前版本和兼容性

## 步骤 6：创建 SKILL.md

领域专业技能使用路由器模式，其基本原则是：

```yaml
---
name: build-{domain-name}
description: Build {domain things} from scratch through shipping. Full lifecycle - build, debug, test, optimize, ship. {Any specific constraints like "CLI-only, no IDE"}.
---

<essential_principles>
## How {This Domain} Works

{Domain-specific principles that ALWAYS apply}

### 1. {First Principle}
{Critical practice that can't be skipped}

### 2. {Second Principle}
{Another fundamental practice}

### 3. {Third Principle}
{Core workflow pattern}
</essential_principles>

<intake>
**Ask the user:**

What would you like to do?
1. Build a new {thing}
2. Debug an existing {thing}
3. Add a feature
4. Write/run tests
5. Optimize performance
6. Ship/release
7. Something else

**Then read the matching workflow from `workflows/` and follow it.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "new", "create", "build", "start" | `workflows/build-new-{thing}.md` |
| 2, "broken", "fix", "debug", "crash", "bug" | `workflows/debug-{thing}.md` |
| 3, "add", "feature", "implement", "change" | `workflows/add-feature.md` |
| 4, "test", "tests", "TDD", "coverage" | `workflows/write-tests.md` |
| 5, "slow", "optimize", "performance", "fast" | `workflows/optimize-performance.md` |
| 6, "ship", "release", "deploy", "publish" | `workflows/ship-{thing}.md` |
| 7, other | Clarify, then select workflow or references |
</routing>

<verification_loop>
## After Every Change

{Domain-specific verification steps}

Example for compiled languages:
```bash

# 1. 它能构建吗？
{构建命令}

# 2. 测试通过了吗？
{测试命令}

# 3.它运行吗？
{运行命令}
```

Report to the user:
- "Build: ✓"
- "Tests: X pass, Y fail"
- "Ready for you to check [specific thing]"
</verification_loop>

<reference_index>
## Domain Knowledge

All in `references/`:

**Architecture:** {list files}
**{Domain Area}:** {list files}
**{Domain Area}:** {list files}
**Development:** {list files}
**Shipping:** {list files}
</reference_index>

<workflows_index>
## Workflows

All in `workflows/`:

| File | Purpose |
|------|---------|
| build-new-{thing}.md | Create new {thing} from scratch |
| debug-{thing}.md | Find and fix bugs |
| add-feature.md | Add to existing {thing} |
| write-tests.md | Write and run tests |
| optimize-performance.md | Profile and speed up |
| ship-{thing}.md | Deploy/distribute |
</workflows_index>
```


## 步骤 7：编写工作流程

对于步骤 3 中确定的每个工作流程：

### 工作流程模板

```markdown
# Workflow: {Workflow Name}

<required_reading>
**Read these reference files NOW before {doing the task}:**
1. references/{relevant-file}.md
2. references/{another-relevant-file}.md
3. references/{third-relevant-file}.md
</required_reading>

<process>
## Step 1: {First Action}

{What to do}

## Step 2: {Second Action}

{What to do - actual implementation steps}

## Step 3: {Third Action}

{What to do}

## Step 4: Verify

{How to prove it works}

```bash

{验证命令}
```
</process>

<anti_patterns>
Avoid:
- {Common mistake 1}
- {Common mistake 2}
- {Common mistake 3}
</anti_patterns>

<success_criteria>
A well-{completed task}:
- {Criterion 1}
- {Criterion 2}
- {Criterion 3}
- Builds/runs without errors
- Tests pass
- Feels {native/professional/correct}
</success_criteria>
```


**关键工作流程特征：**
- 以 required_reading 开头（引用加载）
- 包含实际的实施步骤（不仅仅是“阅读参考资料”）
- 包括验证步骤
- 有成功标准
- 记录反模式

## 步骤 8：撰写综合参考文献

对于步骤 5 中确定的每个参考文件：

### 结构模板

```xml
<overview>
Brief introduction to this domain area
</overview>

<options>
## Available Approaches/Libraries

<option name="Library A">
**When to use:** [specific scenarios]
**Strengths:** [what it's best at]
**Weaknesses:** [what it's not good for]
**Current status:** v{version}, actively maintained
**Learning curve:** [easy/medium/hard]

```code

# 用法示例
```
</option>

<option name="Library B">
[Same structure]
</option>
</options>

<decision_tree>
## Choosing the Right Approach

**If you need [X]:** Use [Library A]
**If you need [Y]:** Use [Library B]
**If you have [constraint Z]:** Use [Library C]

**Avoid [Library D] if:** [specific scenarios]
</decision_tree>

<patterns>
## Common Patterns

<pattern name="Pattern Name">
**Use when:** [scenario]
**Implementation:** [code example]
**Considerations:** [trade-offs]
</pattern>
</patterns>

<anti_patterns>
## What NOT to Do

<anti_pattern name="Common Mistake">
**Problem:** [what people do wrong]
**Why it's bad:** [consequences]
**Instead:** [correct approach]
</anti_pattern>
</anti_patterns>

<platform_considerations>
## Platform-Specific Notes

**Windows:** [considerations]
**macOS:** [considerations]
**Linux:** [considerations]
**Mobile:** [if applicable]
</platform_considerations>
```


### 质量标准

每个参考文献必须包括：
- **当前信息**（验证日期）
- **多个选项**（不仅仅是一个库）
- **决策指导**（何时使用每个）
- **真实示例**（工作代码，而不是伪代码）
- **权衡**（没有灵丹妙药）
- **反模式**（不该做什么）

### 通用参考文件

大多数域需要：
- **architecture.md** - 如何构建项目
- **libraries.md** - 生态系统概述与比较
- **patterns.md** - 特定于域的设计模式
- **testing-debugging.md** - 如何验证正确性
- **性能.md** - 优化策略
- **deployment.md** - 如何运送/分发
- **anti-patterns.md** - 常见错误汇总

## 步骤 9：验证完整性

### 完整性检查表

问：“用户可以仅使用此技能从头开始通过运输来构建专业的{domain thing}吗？”

**必须对以下问题回答“是”：**
- [ ] 涵盖所有主要库/框架？
- [ ] 所有架构方法都有记录吗？
- [ ] 解决了完整的生命周期（构建→调试→测试→优化→发布）？
- [ ] 包括特定于平台的注意事项？
- [ ] 提供“何时使用 X 与 Y”指南？
- [ ] 常见陷阱有记录吗？
- [ ] 目前截至 2024-2025 年？
- [ ] 工作流实际执行任务（不仅仅是参考知识）？
- [ ] 每个工作流程指定要阅读哪些参考文献？

**要检查的具体差距：**
- [ ] 涵盖测试策略？
- [ ] 列出了调试/分析工具？
- [ ] 部署/分发方法已记录？
- [ ] 性能优化已解决？
- [ ] 安全考虑（如果适用）？
- [ ] 资产/资源管理（如果适用）？
- [ ] 网络（如果适用）？

### 双重用途测试

测试两个用例：

**直接调用：**“用户可以调用该技能并构建一些东西吗？”
- 进入适当工作流程的途径
- 工作流程加载相关参考文献
- 工作流程提供实施步骤
- 成功标准明确

**知识参考：**《create-plans可以加载参考来规划项目吗？》
- 参考文献包含决策指导
- 所有选项的比较
- 涵盖完整的生命周期
- 记录架构模式

## 步骤10：创建目录和文件

```bash
# Create structure
mkdir -p ~/.claude/skills/expertise/{domain-name}
mkdir -p ~/.claude/skills/expertise/{domain-name}/workflows
mkdir -p ~/.claude/skills/expertise/{domain-name}/references

# Write SKILL.md
# Write all workflow files
# Write all reference files

# Verify structure
ls -R ~/.claude/skills/expertise/{domain-name}
```


## 步骤 11：在创建计划中记录

更新 `~/.claude/skills/create-plans/SKILL.md` 以引用此新域：

添加到域推断表：
```markdown
| "{keyword}", "{domain term}" | expertise/{domain-name} |
```


因此创建计划可以自动检测并提供加载它。

## 第 12 步：最终质量检查

回顾整个技能：

**技能.md:**
- [ ] 名称与目录匹配 (build-{domain-name})
- [ ] 描述解释了它通过运输从头开始构建东西
- [ ] 内联基本原则（始终加载）
- [ ] 询问用户想要做什么
- [ ] 将映射路由到工作流程
- [ ] 参考索引完整且有条理
- [ ] 工作流程索引完成

**工作流程：**
- [ ] 每个工作流程均以 required_reading 开头
- [ ] 每个工作流程都有实际的实施步骤
- [ ] 每个工作流程都有验证步骤
- [ ] 每个工作流程都有成功标准
- [ ] 工作流程涵盖整个生命周期（构建、调试、测试、优化、交付）

**参考资料：**
- [ ] 纯 XML 结构（无 Markdown 标题）
- [ ] 每个文件中的决策指导
- [ ] 当前版本已验证
- [ ] 代码示例有效
- [ ] 反模式已记录
- [ ] 包括平台注意事项

**完整性：**
- [ ] 专业从业者会发现这很全面
- [ ] 没有缺少主要库/模式
- [ ] 涵盖完整生命周期
- [ ] 通过“从头开始构建到交付”测试
- [ ] 可以由用户直接调用
- [ ] 可以通过知识创建计划加载

</process>

<success_criteria>
当满足以下条件时，领域专业知识技能就完成了：

- [ ] 已完成综合研究（5+ 网络搜索）
- [ ] 所有来源均经过货币验证（2024-2025）
- [ ] 按领域领域组织的知识（不是任意的）
- [ ] SKILL.md 中的基本原则（始终加载）
- [ ] 进入适当工作流程的途径
- [ ]每个工作流程都有必读+实施步骤+验证
- [ ] 每个参考都有决策树和比较
- [ ] 全程记录反模式
- [ ] 涵盖完整生命周期（构建→调试→测试→优化→发布）
- [ ] 包括特定于平台的注意事项
- [ ] 位于 ~/.claude/skills/expertise/{domain-name}/
- [ ] 在 create-plans 域推断表中引用
- [ ] 通过双重用途测试：可以直接调用并加载知识
- [ ] 用户可以通过运输从头开始构建专业的东西
</success_criteria>

<anti_patterns>
**不要：**
- 复制教程内容，无需验证
- 仅包含“入门”材料
- 跳过“何时不使用”指南
- 忘记检查库是否仍在维护
- 按文档类型而不是领域问题进行组织
- 使其仅包含知识，没有执行工作流程
- 跳过工作流程中的验证步骤
- 包括旧博客文章中的过时内容
- 跳过决策树和比较
- 创建仅显示“阅读参考资料”的工作流程

**做：**
- 验证一切都是最新的
- 包括完整的生命周期（建造→运输）
- 提供决策指导
- 记录反模式
- 让工作流程执行真正的任务
- 使用 required_reading 启动工作流程
- 在每个工作流程中包括验证
- 使其详尽，而不是最小化
- 测试直接调用和知识参考用例
</anti_patterns>