---
name: workflows:review
description: 使用多代理分析、超思维和工作树执行详尽的代码审查

argument-hint: "[PR number, GitHub URL, branch name, or latest]"
---
# 查看命令

<command_purpose> 使用多代理分析、超思维和 Git 工作树进行详尽的代码审查，以进行深入的本地检查。 </command_purpose>

## 介绍

<role>高级代码审查架构师，拥有安全、性能、架构和质量保证方面的专业知识</role>

## 先决条件

<requirements>
- 安装并验证了 GitHub CLI (`gh`) 的 Git 存储库
- 清理主/主分支
- 创建工作树和访问存储库的适当权限
- 对于文档审阅：Markdown 文件或文档的路径
</requirements>

## 主要任务

### 1. 确定审核目标和设置（始终是第一位）

<review_target> #$参数</review_target>

<thinking>
首先，我需要确定审查目标类型并设置用于分析的代码。
</thinking>

#### 立即采取行动：

<task_list>

- [ ] 确定审阅类型：PR 编号（数字）、GitHub URL、文件路径 (.md) 或空（当前分支）
- [ ] 检查当前的git分支
- [ ] 如果已经在 PR 分支上 → 继续对当前分支进行分析
- [ ] 如果不同的分支 → 提供使用工作树：“使用 git-worktree 技能进行带有分支名称的隔离调用`skill: git-worktree`
- [ ] 使用 `gh pr view --json` 获取标题、正文、文件、链接问题的 PR 元数据
- [ ] 设置特定语言的分析工具
- [ ] 准备安全扫描环境
- [ ] 确保我们位于正在审查的分支上。使用 gh pr checkout 切换到分支或手动签出分支。

确保代码已准备好进行分析（在工作树中或当前分支上）。然后才继续下一步。

</task_list>

#### 并行代理审查 PR：

<parallel_tasks>

同时运行所有或大部分代理：

1. 任务 kiran-rails-reviewer（PR 内容）
2. 任务 dhh-rails-reviewer（PR 标题）
3.如果使用turbo：任务rails-turbo-expert（PR内容）
4. 任务 git-history-analyzer(PR 内容)
5.任务依赖-侦探（PR内容）
6.任务模式-识别-专家（PR内容）
7.任务架构-策略师（PR内容）
8.任务代码-哲学家（PR内容）
9.任务安全-哨兵（PR内容）
10.任务绩效-oracle（PR内容）
11.任务devops-harmony-analyst（PR内容）
12.任务数据完整性守护者（PR内容）
13. 任务agent-native-reviewer（PR内容）-验证新功能是否可供代理访问

</parallel_tasks>

#### 条件代理（如果适用则运行）：

<conditional_agents>

仅当 PR 符合特定条件时才会运行这些代理。检查 PR 文件列表以确定它们是否适用：

**如果 PR 包含数据库迁移（db/migrate/*.rb 文件）或数据回填：**

14. 任务 data-migration-expert(PR 内容) - 验证 ID 映射是否与生产匹配、检查交换值、验证回滚安全性
15. 任务部署验证代理（PR 内容） - 使用 SQL 验证查询创建 Go/No-Go 部署清单

**何时运行迁移代理：**
- PR 包括匹配 `db/migrate/*.rb` 的文件
- PR 修改存储 ID、枚举或映射的列
- PR 包括数据回填脚本或 rake 任务
- PR 更改数据读取/写入的方式（例如，从 FK 更改为字符串列）
- PR 标题/正文提及：迁移、回填、数据转换、ID 映射

**这些代理检查的内容：**
- `data-migration-expert`：验证硬编码映射与生产现实是否匹配（防止交换 ID）、检查孤立关联、验证双写入模式
- `deployment-verification-agent`：使用 SQL 查询、回滚过程和监控计划生成可执行的部署前/部署后清单

</conditional_agents>

### 4. 超思维深度探索阶段

<ultrathink_instruction> 对于以下每个阶段，花费最大的认知努力。一步步思考。考虑所有角度。质疑假设。并将所有评论综合提供给用户。</ultrathink_instruction>

<deliverable>
具有组件交互的完整系统上下文图
</deliverable>

#### 第三阶段：利益相关者视角分析

<thinking_prompt> 超思维：设身处地为每个利益相关者着想。对他们来说什么重要？他们的痛点是什么？ </thinking_prompt>

<stakeholder_perspectives>

1. **开发者视角** <questions>

- 这是否容易理解和修改？
   - API 直观吗？
   - 调试简单吗？
   - 我可以轻松测试吗？ </questions>

2. **运营视角** <questions>

- 我如何安全地部署它？
   - 有哪些可用的指标和日志？
   - 如何解决问题？
   - 资源要求是什么？ </questions>

3. **最终用户视角** <questions>

- 该功能直观吗？
   - 错误消息有帮助吗？
   - 表现是否可以接受？
   - 它能解决我的问题吗？ </questions>

4. **安全团队观点** <questions>

- 攻击面是什么？
   - 是否有合规要求？
   - 数据如何受到保护？
   - 审计能力有哪些？ </questions>

5. **商业视角** <questions>
   - 投资回报率是多少？
   - 是否存在法律/合规风险？
   - 这对上市时间有何影响？
   - 总拥有成本是多少？ </questions></stakeholder_perspectives>

#### 第四阶段：场景探索

<thinking_prompt> ULTRA-THINK：探索边缘情况和故障场景。可能会出什么问题？系统在压力下表现如何？ </thinking_prompt>

<scenario_checklist>

- [ ] **Happy Path**：有效输入的正常操作
- [ ] **无效输入**：空、空、格式错误的数据
- [ ] **边界条件**：最小/最大值，空集合
- [ ] **并发访问**：竞争条件、死锁
- [ ] **规模测试**：10x、100x、1000x 正常负载
- [ ] **网络问题**：超时、部分失败
- [ ] **资源耗尽**：内存、磁盘、连接
- [ ] **安全攻击**：注入、溢出、DoS
- [ ] **数据损坏**：部分写入、不一致
- [ ] **级联故障**：下游服务问题</scenario_checklist>

### 6. 多角度审查视角

#### 技术卓越角度

- 代码工艺评估
- 工程最佳实践
- 技术文件质量
- 工具和自动化评估

#### 商业价值角度

- 功能完整性验证
- 对用户的性能影响
- 成本效益分析
- 上市时间考虑因素

#### 风险管理角度

- 安全风险评估
- 操作风险评估
- 合规风险核查
- 技术债务积累

#### 团队动力角度

- 代码审查礼仪
- 知识共享的有效性
- 协作模式
- 指导机会

### 4. 简化和极简主义回顾

运行任务 code-simplicity-reviewer() 看看我们是否可以简化代码。

### 5. 使用 file-todos 技能进行调查结果综合和 Todo 创建

<critical_requirement> 所有结果必须使用 file-todos 技能存储在 todos/ 目录中。综合后立即创建待办事项文件 - 不要首先提供结果供用户批准。使用结构化待办事项管理技能。 </critical_requirement>

#### 第 1 步：综合所有发现

<thinking>
将所有代理报告合并到分类的结果列表中。
删除重复项，按严重性和影响确定优先级。
</thinking>

<synthesis_tasks>

- [ ] 收集所有并行代理的结果
- [ ] 按类型分类：安全、性能、架构、质量等。
- [ ] 指定严重性级别：🔴 严重 (P1)、🟡 重要 (P2)、🔵 最好有 (P3)
- [ ] 删除重复或重叠的结果
- [ ] 估计每个发现的工作量（小/中/大）

</synthesis_tasks>

#### 步骤 2：使用 file-todos 技能创建 Todo 文件

<critical_instruction> 使用文件待办事项技能立即为所有发现创建待办事项文件。不要一一呈现调查结果并请求用户批准。使用该技能并行创建所有待办事项文件，然后将结果汇总给用户。 </critical_instruction>

**实施选项：**

**选项 A：直接创建文件（快速）**

- 使用Write工具直接创建todo文件
- 所有调查结果并行以提高速度
- 使用`.claude/skills/file-todos/assets/todo-template.md`中的标准模板
- 遵循命名约定：`{issue_id}-pending-{priority}-{description}.md`

**选项 B：并行子代理（推荐用于规模）** 对于具有 15 个以上发现结果的大型 PR，请使用子代理并行创建发现文件：

```bash
# Launch multiple finding-creator agents in parallel
Task() - Create todos for first finding
Task() - Create todos for second finding
Task() - Create todos for third finding
etc. for each finding.
```


子代理可以：

- 同时处理多个发现
- 编写详细的待办事项文件，并填写所有部分
- 按严重程度组织调查结果
- 创建全面的建议解决方案
- 添加验收标准和工作日志
- 完成速度比顺序处理快得多

**执行策略：**

1. 将所有发现综合为类别（P1/P2/P3）
2. 按严重程度对结果进行分组
3. 启动 3 个并行子代理（每个严重级别一个）
4. 每个子代理使用 file-todos 技能创建一批待办事项
5. 整合结果并提出总结

**流程（使用file-todos技巧）：**

1. 对于每项发现：

- 确定严重性（P1/P2/P3）
   - 写出详细的问题陈述和调查结果
   - 创建 2-3 个包含优点/缺点/努力/风险的建议解决方案
   - 估计工作量（小/中/大）
   - 添加验收标准和工作日志

2. 使用 file-todos 技能进行结构化的待办事项管理：

   ```bash
   skill: file-todos
   ```


该技能提供：

- 模板位置：`.claude/skills/file-todos/assets/todo-template.md`
   - 命名约定：`{issue_id}-{status}-{priority}-{description}.md`
   - YAML frontmatter 结构：状态、优先级、issue_id、标签、依赖项
   - 所有必填部分：问题陈述、发现、解决方案等。

3. 并行创建todo文件：

   ```bash
   {next_id}-pending-{priority}-{description}.md
   ```


4. 示例：

   ```
   001-pending-p1-path-traversal-vulnerability.md
   002-pending-p1-api-response-validation.md
   003-pending-p2-concurrency-limit.md
   004-pending-p3-unused-parameter.md
   ```


5. 遵循文件待办事项技能中的模板结构：`.claude/skills/file-todos/assets/todo-template.md`

**Todo 文件结构（来自模板）：**

每个待办事项必须包括：

- **YAML frontmatter**：状态、优先级、issue_id、标签、依赖项
- **问题陈述**：什么损坏/丢失，为什么重要
- **发现**：代理的发现以及证据/位置
- **建议的解决方案**：2-3 个选项，每个选项都有优点/缺点/努力/风险
- **建议采取的行动**：（在分类期间填写，最初留空）
- **技术细节**：受影响的文件、组件、数据库更改
- **验收标准**：可测试的清单项目
- **工作日志**：包含行动和学习内容的日期记录
- **资源**：PR、问题、文档、类似模式的链接

**文件命名约定：**

```
{issue_id}-{status}-{priority}-{description}.md

Examples:
- 001-pending-p1-security-vulnerability.md
- 002-pending-p2-performance-optimization.md
- 003-pending-p3-code-cleanup.md
```


**状态值：**

- `pending` - 新发现，需要分类/决策
- `ready` - 经经理批准，准备工作
- `complete` - 工作完成

**优先级值：**

- `p1` - 严重（块合并、安全/数据问题）
- `p2` - 重要（应修复，建筑/性能）
- `p3` - 锦上添花（增强、清理）

**标记：** 始终添加 `code-review` 标签，加上：`security`、`performance`、`architecture`、`rails`、`quality` 等。

#### 步骤 3：总结报告

创建所有待办事项文件后，呈现全面的摘要：

````markdown
## ✅ Code Review Complete

**Review Target:** PR #XXXX - [PR Title] **Branch:** [branch-name]

### Findings Summary:

- **Total Findings:** [X]
- **🔴 CRITICAL (P1):** [count] - BLOCKS MERGE
- **🟡 IMPORTANT (P2):** [count] - Should Fix
- **🔵 NICE-TO-HAVE (P3):** [count] - Enhancements

### Created Todo Files:

**P1 - Critical (BLOCKS MERGE):**

- `001-pending-p1-{finding}.md` - {description}
- `002-pending-p1-{finding}.md` - {description}

**P2 - Important:**

- `003-pending-p2-{finding}.md` - {description}
- `004-pending-p2-{finding}.md` - {description}

**P3 - Nice-to-Have:**

- `005-pending-p3-{finding}.md` - {description}

### Review Agents Used:

- kieran-rails-reviewer
- security-sentinel
- performance-oracle
- architecture-strategist
- agent-native-reviewer
- [other agents]

### Next Steps:

1. **Address P1 Findings**: CRITICAL - must be fixed before merge

   - Review each P1 todo in detail
   - Implement fixes or request exemption
   - Verify fixes before merging PR

2. **Triage All Todos**:
   ```bash
   ls todos/*-pending-*.md  # View all pending todos
   /triage                  # Use slash command for interactive triage
   ```
````


3. **处理批准的待办事项**：

   ```bash
   /resolve_todo_parallel  # Fix all approved items efficiently
   ```


4. **跟踪进度**：
   - 状态更改时重命名文件：待处理 → 准备就绪 → 完成
   - 在工作时更新工作日志
   - 提交待办事项：`git add todos/ && git commit -m "refactor: add code review findings"`

### 严重程度细分：

**🔴 P1（关键 - 块合并）：**

- 安全漏洞
- 数据损坏风险
- 重大变化
- 关键的架构问题

**🟡 P2（重要 - 应该修复）：**

- 性能问题
- 重大的架构问题
- 主要代码质量问题
- 可靠性问题

**🔵 P3（必备）：**

- 小改进
- 代码清理
- 优化机会
- 文档更新

```

### 7. End-to-End Testing (Optional)

<detect_project_type>

**First, detect the project type from PR files:**

| Indicator | Project Type |
|-----------|--------------|
| `*.xcodeproj`, `*.xcworkspace`, `Package.swift` (iOS) | iOS/macOS |
| `Gemfile`, `package.json`, `app/views/*`, `*.html.*` | Web |
| Both iOS files AND web files | Hybrid (test both) |

</detect_project_type>

<offer_testing>

After presenting the Summary Report, offer appropriate testing based on project type:

**For Web Projects:**
```markdown

**“想要在受影响的页面上运行 Playwright 浏览器测试吗？”**
1. 是 - 运行`/playwright-test`
2. 否 - 跳过
```

**For iOS Projects:**
```markdown

**“想要在应用程序上运行 Xcode 模拟器测试吗？”**
1. 是 - 运行`/xcode-test`
2. 否 - 跳过
```

**For Hybrid Projects (e.g., Rails + Hotwire Native):**
```markdown

**“想要运行端到端测试吗？”**
1. 仅限网络 - 运行`/playwright-test`
2. 仅限 iOS - 运行`/xcode-test`
3. 两者 - 运行两个命令
4. 否 - 跳过
```

</offer_testing>

#### If User Accepts Web Testing:

Spawn a subagent to run Playwright tests (preserves main context):

```

通用任务（“针对 PR #[number] 运行 /playwright-test。测试所有受影响的页面，检查控制台错误，通过创建待办事项和修复来处理故障。”）
```

The subagent will:
1. Identify pages affected by the PR
2. Navigate to each page and capture snapshots
3. Check for console errors
4. Test critical interactions
5. Pause for human verification on OAuth/email/payment flows
6. Create P1 todos for any failures
7. Fix and retry until all tests pass

**Standalone:** `/playwright-test [PR number]`

#### If User Accepts iOS Testing:

Spawn a subagent to run Xcode tests (preserves main context):

```

通用任务（“针对方案 [名称] 运行 /xcode-test。为模拟器构建、安装、启动、截屏、检查崩溃。”）
```

The subagent will:
1. Verify XcodeBuildMCP is installed
2. Discover project and schemes
3. Build for iOS Simulator
4. Install and launch app
5. Take screenshots of key screens
6. Capture console logs for errors
7. Pause for human verification (Sign in with Apple, push, IAP)
8. Create P1 todos for any failures
9. Fix and retry until all tests pass

**Standalone:** `/xcode-test [scheme]`

### Important: P1 Findings Block Merge

Any **🔴 P1 (CRITICAL)** findings must be addressed before merging the PR. Present these prominently and ensure they're resolved before accepting the PR.
```
