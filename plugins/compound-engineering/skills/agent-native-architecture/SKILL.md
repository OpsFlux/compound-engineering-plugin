---
name: agent-native-architecture
description: 当使用提示本机架构构建人工智能代理时，应该使用此技能，其中功能是在提示中定义的，而不是代码。在创建自主代理、设计 MCP 服务器、实现自我修改系统或采用“信任代理的智能”理念时使用它。

---
<essential_principles>
## 及时本土哲学

代理原生工程颠覆了传统的软件架构。您无需编写代理执行的代码，而是在提示中定义结果并让代理弄清楚如何实现它们。

### 基本原则

**用户能做的，代理也能做。很多事情开发商可以做，代理商也可以做。**

不要人为地限制代理。如果用户可以读取文件、编写代码、浏览网页、部署应用程序，那么代理也应该能够执行这些操作。代理人弄清楚如何实现结果；它不只是调用您预先编写的函数。

### 功能是提示

每个功能都是一个提示，定义结果并为代理提供所需的工具。然后代理就会弄清楚如何完成它。

**传统：** 功能 = 代理调用的代码库中的函数
**提示原生：** 功能 = 提示定义所需结果 + 原始工具

代理不会执行您的代码。它使用原语来实现您描述的结果。

### 工具提供的是能力，而不是行为

工具应该是能够实现功能的原语。提示定义了如何使用该功能。

**错误：** `generate_dashboard(data, layout, filters)` — 代理执行您的工作流程
**右：** `read_file`、`write_file`、`list_files` — 代理弄清楚如何构建仪表板

纯原语更好，但如果域原语（如`store_feedback`）不编码逻辑（仅存储/检索），那么它们也可以。

### 开发生命周期

1. **从提示开始** - 新功能以自然语言定义结果开始
2. **快速迭代** - 通过编辑散文来改变行为，而不是重构代码
3. **稳定时毕业** - 当需求稳定且速度/可靠性很重要时，强化编码
4. **许多功能保留为提示** - 并非所有内容都需要成为代码

### 自我修改（高级）

高级层：可以发展自己的代码、提示和行为的代理。并非每个应用程序都需要，但未来的很大一部分。

实施时：
- 代码变更的审批门
- 修改前自动提交（回滚能力）
- 更改后的健康检查
- 重启前构建验证

### 何时不使用此方法

- **高频操作** - 每秒数千次调用
- **确定性要求** - 每次都输出完全相同
- **成本敏感场景** - 当 API 成本过高时
- **高安全性** - 尽管这对于大多数应用程序来说都被夸大了
</essential_principles>

<intake>
您需要代理本机架构的哪些方面的帮助？

1. **设计架构** - 规划一个新的提示原生代理系统
2. **创建 MCP 工具** - 遵循以下理念构建原始工具
3. **编写系统提示** - 在提示中定义座席行为
4. **自我修改** - 使智能体能够安全地自我进化
5. **审查/重构** - 使现有代码更加提示原生
6. **上下文注入** - 将运行时应用程序状态注入代理提示中
7. **操作对等** - 确保代理可以做用户可以做的所有事情
8. **共享工作空间** - 在同一数据空间中设置代理和用户
9. **测试** - 测试代理本机应用程序的功能和奇偶校验
10. **移动模式** - 处理后台执行、权限、成本
11. **API集成** - 连接到外部API（HealthKit、HomeKit、GraphQL）

**等待回复后再继续。**
</intake>

<routing>
|回应 |行动|
|----------|--------|
| 1、“设计”、“建筑”、“规划”|阅读 [architecture-patterns.md](./references/architecture-patterns.md)，然后应用下面的架构清单 |
| 2、“工具”、“mcp”、“原始”|阅读 [mcp-tool-design.md](./references/mcp-tool-design.md) |
| 3、“提示”、“系统提示”、“行为”|阅读 [system-prompt-design.md](./references/system-prompt-design.md) |
| 4、“自我修改”、“进化”、“git”|阅读[自我修改.md](./references/self-modification.md) |
| 5、“审查”、“重构”、“现有”|阅读 [refactoring-to-prompt-native.md](./references/refactoring-to-prompt-native.md) |
| 6、“上下文”、“注入”、“运行时”、“动态”|阅读 [dynamic-context-injection.md](./references/dynamic-context-injection.md) |
| 7、“比价”、“ui操作”、“能力图”|阅读 [action-parity-discipline.md](./references/action-parity-discipline.md) |
| 8、“工作空间”、“共享”、“文件”、“文件系统”|阅读 [shared-workspace-architecture.md](./references/shared-workspace-architecture.md) |
| 9、“测试”、“测试”、“验证”、“验证”|阅读 [agent-native-testing.md](./references/agent-native-testing.md) |
| 10、“移动”、“ios”、“android”、“背景”|阅读 [mobile-patterns.md](./references/mobile-patterns.md) |
| 11、“api”、“healthkit”、“homekit”、“graphql”、“外部”|阅读 [mcp-tool-design.md](./references/mcp-tool-design.md)（动态能力发现部分）|

**阅读参考资料后，将这些模式应用到用户的特定上下文中。**
</routing>

<architecture_checklist>
## 架构审查清单（在设计期间应用）

在设计原生代理系统时，请在**实施之前验证这些**：

### 工具设计
- [ ] **动态与静态：** 对于代理应具有完全用户级访问权限的外部 API（HealthKit、HomeKit、GraphQL），请使用动态功能发现。仅当有意限制代理范围时才使用静态映射。
- [ ] **CRUD 完整性：** 每个实体都有创建、读取、更新和删除工具
- [ ] **原语而非工作流程：**工具启用功能，它们不编码业务逻辑
- [ ] **API 作为验证器：** 在 API 验证时使用 `z.string()` 输入，而不是 `z.enum()`

### 行动平等
- [ ] **能力图：** 每个UI动作都有对应的代理工具
- [ ] **编辑/删除：** 如果 UI 可以编辑或删除，代理也必须能够
- [ ] **写入测试：** “向 [应用程序位置] 写入内容”必须适用于所有位置

### 用户界面集成
- [ ] **代理 → UI：** 定义代理更改如何反映在 UI 中（共享服务、文件监视或事件总线）
- [ ] **无静默操作：** 代理写入应立即触发 UI 更新
- [ ] **能力发现：** 用户可以了解代理可以做什么（入门、提示）

### 上下文注入
- [ ] **可用资源：**系统提示包括存在的资源（文件、数据、类型）
- [ ] **可用功能：** 系统提示记录代理可以对用户词汇执行哪些操作
- [ ] **动态上下文：** 长时间会话的上下文刷新（或提供`refresh_context`工具）

### 手机（如果适用）
- [ ] **后台执行：** iOS 应用程序暂停的检查点/恢复模式
- [ ] **权限：** 工具中的即时权限请求
- [ ] **成本意识：** 模型层选择（俳句/十四行诗/作品）

**设计架构时，明确解决计划中的每个复选框。**
</architecture_checklist>

<quick_start>
分三步构建提示原生代理：

**第 1 步：定义原始工具**
```typescript
const tools = [
  tool("read_file", "Read any file", { path: z.string() }, ...),
  tool("write_file", "Write any file", { path: z.string(), content: z.string() }, ...),
  tool("list_files", "List directory", { path: z.string() }, ...),
];
```


**第二步：在系统提示符中写入行为**
```markdown
## Your Responsibilities
When asked to organize content, you should:
1. Read existing files to understand the structure
2. Analyze what organization makes sense
3. Create appropriate pages using write_file
4. Use your judgment about layout and formatting

You decide the structure. Make it good.
```


**第3步：让代理工作**
```typescript
query({
  prompt: userMessage,
  options: {
    systemPrompt,
    mcpServers: { files: fileServer },
    permissionMode: "acceptEdits",
  }
});
```

</quick_start>

<reference_index>
## 领域知识

`references/`中的所有参考文献：

**核心模式：**
- **架构：** [architecture-patterns.md](./references/architecture-patterns.md)
- **工具设计：** [mcp-tool-design.md](./references/mcp-tool-design.md) - 包括动态功能发现、CRUD 完整性
- **提示：** [system-prompt-design.md](./references/system-prompt-design.md)
- **自我修改：** [自我修改.md](./references/self-modification.md)
- **重构：** [refactoring-to-prompt-native.md](./references/refactoring-to-prompt-native.md)

**代理本土学科：**
- **上下文注入：** [dynamic-context-injection.md](./references/dynamic-context-injection.md)
- **动作奇偶校验：** [action-parity-discipline.md](./references/action-parity-discipline.md)
- **共享工作空间：** [shared-workspace-architecture.md](./references/shared-workspace-architecture.md)
- **测试：** [agent-native-testing.md](./references/agent-native-testing.md)
- **移动模式：** [mobile-patterns.md](./references/mobile-patterns.md)
</reference_index>

<anti_patterns>
## 不该做什么

**大罪：代理执行您的代码而不是弄清楚事情**

这是最常见的错误。您重新编写工作流代码并让代理调用它，而不是定义结果并让代理弄清楚如何操作。

```typescript
// WRONG - You wrote the workflow, agent just executes it
tool("process_feedback", async ({ message }) => {
  const category = categorize(message);      // Your code
  const priority = calculatePriority(message); // Your code
  await store(message, category, priority);   // Your code
  if (priority > 3) await notify();           // Your code
});

// RIGHT - Agent figures out how to process feedback
tool("store_item", { key, value }, ...);  // Primitive
tool("send_message", { channel, content }, ...);  // Primitive
// Prompt says: "Rate importance 1-5 based on actionability, store feedback, notify if >= 4"
```


**不要人为地限制代理可以做什么**

如果用户能做到，代理也应该能做到。

```typescript
// WRONG - limiting agent capabilities
tool("read_approved_files", { path }, async ({ path }) => {
  if (!ALLOWED_PATHS.includes(path)) throw new Error("Not allowed");
  return readFile(path);
});

// RIGHT - give full capability, use guardrails appropriately
tool("read_file", { path }, ...);  // Agent can read anything
// Use approval gates for writes, not artificial limits on reads
```


**不要在工具中编码决策**
```typescript
// Wrong - tool decides format
tool("format_report", { format: z.enum(["markdown", "html", "pdf"]) }, ...)

// Right - agent decides format via prompt
tool("write_file", ...) // Agent chooses what to write
```


**不要在提示中过度指定**
```markdown
// Wrong - micromanaging the HOW
When creating a summary, use exactly 3 bullet points,
each under 20 words, formatted with em-dashes...

// Right - define outcome, trust intelligence
Create clear, useful summaries. Use your judgment.
```


### 代理本机反模式

**上下文饥饿**
代理不知道应用程序中存在哪些资源。
```
User: "Write something about Catherine the Great in my feed"
Agent: "What feed? I don't understand what system you're referring to."
```

修复：在运行时将可用的资源、功能和词汇注入到系统提示中。

**孤儿功能**
没有等效代理的 UI 操作。
```swift
// UI has a "Publish to Feed" button
Button("Publish") { publishToFeed(insight) }
// But no agent tool exists to do the same thing
```

修复：为每个UI操作在系统提示中添加相应的工具和文档。

**沙箱隔离**
代理在与用户不同的数据空间中工作。
```
Documents/
├── user_files/        ← User's space
└── agent_output/      ← Agent's space (isolated)
```

修复：使用共享工作区，代理和用户都对同一文件进行操作。

**无声的行动**
代理更改状态但 UI 不更新。
```typescript
// Agent writes to database
await db.insert("feed", content);
// But UI doesn't observe this table - user sees nothing
```

修复：使用具有反应式绑定或文件系统观察的共享数据存储。

**能力隐藏**
用户无法发现代理可以做什么。
```
User: "Help me with my reading"
Agent: "What would you like help with?"
// Agent doesn't mention it can publish to feed, research books, etc.
```

修复：在代理响应中包含功能提示或提供入门指导。

**静态工具映射（适用于代理本机应用程序）**
当您希望代理拥有完全访问权限时，为每个 API 端点构建单独的工具。
```typescript
// You built 50 tools for 50 HealthKit types
tool("read_steps", ...)
tool("read_heart_rate", ...)
tool("read_sleep", ...)
// When glucose tracking is added... code change required
// Agent can only access what you anticipated
```

修复：使用动态功能发现 - 一种用于发现可用内容的 `list_*` 工具，一种用于访问任何类型的通用工具。请参阅 [mcp-tool-design.md](./references/mcp-tool-design.md)。 （注意：静态映射适用于有意限制范围的受约束代理。）

**不完整的CRUD**
代理可以创建但不能更新或删除。
```typescript
// ❌ User: "Delete that journal entry"
// Agent: "I don't have a tool for that"
tool("create_journal_entry", ...)
// Missing: update_journal_entry, delete_journal_entry
```

修复：每个实体都需要完整的 CRUD（创建、读取、更新、删除）。 CRUD 审核：对于每个实体，验证所有四个操作是否存在。
</anti_patterns>

<success_criteria>
在以下情况下，您已经构建了提示本机代理：

**核心提示-母语标准：**
- [ ] 代理找出如何实现结果，而不仅仅是调用您的函数
- [ ] 用户能做什么，代理就能做什么（没有人为限制）
- [ ] 功能是定义结果的提示，而不是定义工作流程的代码
- [ ] 工具是启用功能的原语（读、写、存储、调用 API）
- [ ] 改变行为意味着编辑散文，而不是重构代码
- [ ] 代理可以用你意想不到的巧妙方法给你带来惊喜
- [ ] 您可以通过编写新的提示部分而不是新代码来添加新功能

**工具设计标准：**
- [ ] 外部 API（代理应具有完全访问权限）使用动态功能发现
- [ ] 每个实体都有完整的 CRUD（创建、读取、更新、删除）
- [ ] API 验证输入，而不是您的枚举定义
- [ ] 每个 API 表面都存在发现工具（`list_*`、`discover_*`）

**代理本地人标准：**
- [ ] 系统提示包括有关应用程序状态的动态上下文（可用资源、最近的活动）
- [ ] 每个UI动作都有对应的代理工具（动作奇偶校验）
- [ ] 代理工具以用户词汇记录在系统提示中
- [ ] 代理和用户在同一数据空间（共享工作空间）中工作
- [ ] 代理操作立即反映在 UI 中（共享服务、文件监视或事件总线）
- [ ] “向 [应用程序位置] 写入内容”测试在所有位置均通过
- [ ] 用户可以发现代理可以做什么（功能提示、入门）
- [ ] 长时间会话的上下文刷新（或存在`refresh_context`工具）

**特定于移动设备的标准（如果适用）：**
- [ ] 实现后台执行处理（检查点/恢复）
- [ ] 在工具中妥善处理权限请求
- [ ] 成本意识设计（适当的模型层、批处理）
</success_criteria>