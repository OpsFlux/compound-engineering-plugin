---
name: deepen-plan
description: 与每个部分的并行研究代理一起增强计划，以增加深度、最佳实践和实施细节

argument-hint: "[path to plan file]"
---
# 深化计划-力量增强模式

## 介绍

**注意：当前年份是 2025 年。** 在搜索最新文档和最佳实践时使用此选项。

该命令采用现有计划（来自`/workflows:plan`）并通过并行研究代理增强每个部分。每个主要元素都有自己专门的研究子代理来寻找：
- 最佳实践和行业模式
- 性能优化
- UI/UX 改进（如果适用）
- 质量增强和边缘情况
- 现实世界的实施示例

其结果是一个根深蒂固、可投入生产的计划以及具体的实施细节。

## 计划文件

<plan_path> #$参数</plan_path>

**如果上面的计划路径为空：**
1. 检查最近的计划：`ls -la plans/`
2. 询问用户：“您想深化哪个计划？请提供路径（例如`plans/my-feature.md`）。”

在获得有效的计划文件路径之前不要继续。

## 主要任务

### 1.解析和分析计划结构

<thinking>
首先，阅读并解析该计划，以确定可以通过研究增强的每个主要部分。
</thinking>

**阅读计划文件并摘录：**
- [ ] 概述/问题陈述
- [ ] 提议的解决方案部分
- [ ] 技术方法/架构
- [ ] 实施阶段/步骤
- [ ] 代码示例和文件参考
- [ ] 验收标准
- [ ] 提到的任何 UI/UX 组件
- [ ] 提到的技术/框架（Rails、React、Python、TypeScript 等）
- [ ] 领域领域（数据模型、API、UI、安全性、性能等）

**创建部分清单：**
```
Section 1: [Title] - [Brief description of what to research]
Section 2: [Title] - [Brief description of what to research]
...
```


### 2.发现并应用可用技能

<thinking>
动态发现所有可用技能并将其与计划部分相匹配。不要假设存在哪些技能 - 在运行时发现它们。
</thinking>

**第 1 步：从所有来源发现所有可用技能**

```bash
# 1. Project-local skills (highest priority - project-specific)
ls .claude/skills/

# 2. User's global skills (~/.claude/)
ls ~/.claude/skills/

# 3. compound-engineering plugin skills
ls ~/.claude/plugins/cache/*/compound-engineering/*/skills/

# 4. ALL other installed plugins - check every plugin for skills
find ~/.claude/plugins/cache -type d -name "skills" 2>/dev/null

# 5. Also check installed_plugins.json for all plugin locations
cat ~/.claude/plugins/installed_plugins.json
```


**重要：**检查每个来源。不要假设复合工程是唯一的插件。使用任何已安装的相关插件中的技能。

**第 2 步：对于每个发现的技能，请阅读其 SKILL.md 以了解其作用**

```bash
# For each skill directory found, read its documentation
cat [skill-path]/SKILL.md
```


**第 3 步：将技能与规划内容相匹配**

对于发现的每项技能：
- 阅读其 SKILL.md 描述
- 检查是否有任何计划部分与技能领域相匹配
- 如果匹配，则生成一个子代理来应用该技能的知识

**第 4 步：为每个匹配的技能生成一个子代理**

**关键：对于每个匹配的技能，生成一个单独的子代理并指示它使用该技能。**

对于每个匹配的技能：
```
Task general-purpose: "You have the [skill-name] skill available at [skill-path].

YOUR JOB: Use this skill on the plan.

1. Read the skill: cat [skill-path]/SKILL.md
2. Follow the skill's instructions exactly
3. Apply the skill to this content:

[relevant plan section or full plan]

4. Return the skill's full output

The skill tells you what to do - follow it. Execute the skill completely."
```


**并行生成所有技能子代理：**
- 每个匹配技能 1 个子代理
- 每个子代理阅读并使用其分配的技能
- 全部同时运行
- 10、20、30个技能的副代理就可以了

**各分代理：**
1.读取其技能的SKILL.md
2. 遵循技能的工作流程/说明
3. 将技能应用到计划中
4. 返回技能产生的任何内容（代码、建议、模式、评论等）

**生成示例：**
```
Task general-purpose: "Use the dhh-rails-style skill at ~/.claude/plugins/.../dhh-rails-style. Read SKILL.md and apply it to: [Rails sections of plan]"

Task general-purpose: "Use the frontend-design skill at ~/.claude/plugins/.../frontend-design. Read SKILL.md and apply it to: [UI sections of plan]"

Task general-purpose: "Use the agent-native-architecture skill at ~/.claude/plugins/.../agent-native-architecture. Read SKILL.md and apply it to: [agent/tool sections of plan]"

Task general-purpose: "Use the security-patterns skill at ~/.claude/skills/security-patterns. Read SKILL.md and apply it to: [full plan]"
```


**对子代理的技能没有限制。为每一项可能相关的技能生成一个。**

### 3.发现并应用所学到的知识/解决方案

<thinking>
检查 /workflows:compound 中记录的学习内容。这些已解决的问题存储为 Markdown 文件。为每个学习生成一个子代理来检查它是否相关。
</thinking>

**学习位置 - 检查这些确切的文件夹：**

```
docs/solutions/           <-- PRIMARY: Project-level learnings (created by /workflows:compound)
├── performance-issues/
│   └── *.md
├── debugging-patterns/
│   └── *.md
├── configuration-fixes/
│   └── *.md
├── integration-issues/
│   └── *.md
├── deployment-issues/
│   └── *.md
└── [other-categories]/
    └── *.md
```


**第 1 步：查找所有学习 Markdown 文件**

运行以下命令来获取每个学习文件：

```bash
# PRIMARY LOCATION - Project learnings
find docs/solutions -name "*.md" -type f 2>/dev/null

# If docs/solutions doesn't exist, check alternate locations:
find .claude/docs -name "*.md" -type f 2>/dev/null
find ~/.claude/docs -name "*.md" -type f 2>/dev/null
```


**第2步：阅读每个学习的frontmatter以进行过滤**

每个学习文件都有带有元数据的 YAML frontmatter。读取每个文件的前 20 行以获得：

```yaml
---
title: "N+1 Query Fix for Briefs"
category: performance-issues
tags: [activerecord, n-plus-one, includes, eager-loading]
module: Briefs
symptom: "Slow page load, multiple queries in logs"
root_cause: "Missing includes on association"
---
```


**对于每个 .md 文件，快速扫描其 frontmatter：**

```bash
# Read first 20 lines of each learning (frontmatter + summary)
head -20 docs/solutions/**/*.md
```


**第 3 步：过滤 - 只为可能相关的学习生成子代理**

将每个学习的前沿内容与计划进行比较：
- `tags:` - 任何标签是否与计划中的技术/模式相匹配？
- `category:` - 此类别相关吗？ （例如，如果计划仅包含 UI，则跳过部署问题）
- `module:` - 该计划是否涉及此模块？
- `symptom:` / `root_cause:` - 该计划会出现此问题吗？

**跳过明显不适用的学习内容：**
- 计划仅限前端 → 跳过 `database-migrations/` 学习内容
- 计划是 Python → 跳过 `rails-specific/` 学习
- 计划没有授权 → 跳过 `authentication-issues/` 学习内容

**生成子代理以获取可能适用的学习内容：**
- 任何标签与计划技术重叠
- 与计划域相同的类别
- 类似的模式或担忧

**步骤 4：生成子代理以进行过滤学习**

对于通过过滤器的每个学习：

```
Task general-purpose: "
LEARNING FILE: [full path to .md file]

1. Read this learning file completely
2. This learning documents a previously solved problem

Check if this learning applies to this plan:

---
[full plan content]
---

If relevant:
- Explain specifically how it applies
- Quote the key insight or solution
- Suggest where/how to incorporate it

If NOT relevant after deeper analysis:
- Say 'Not applicable: [reason]'
"
```


**过滤示例：**
```
# Found 15 learning files, plan is about "Rails API caching"

# SPAWN (likely relevant):
docs/solutions/performance-issues/n-plus-one-queries.md      # tags: [activerecord] ✓
docs/solutions/performance-issues/redis-cache-stampede.md    # tags: [caching, redis] ✓
docs/solutions/configuration-fixes/redis-connection-pool.md  # tags: [redis] ✓

# SKIP (clearly not applicable):
docs/solutions/deployment-issues/heroku-memory-quota.md      # not about caching
docs/solutions/frontend-issues/stimulus-race-condition.md    # plan is API, not frontend
docs/solutions/authentication-issues/jwt-expiry.md           # plan has no auth
```


**为所有过滤的学习并行生成子代理。**

**这些知识是机构知识 - 应用它们可以防止重复过去的错误。**

### 4.启动每部分研究代理

<thinking>
对于计划中的每个主要部分，派生专门的子代理来研究改进。使用探索代理类型进行开放式研究。
</thinking>

**对于每个确定的部分，启动并行研究：**

```
Task Explore: "Research best practices, patterns, and real-world examples for: [section topic].
Find:
- Industry standards and conventions
- Performance considerations
- Common pitfalls and how to avoid them
- Documentation and tutorials
Return concrete, actionable recommendations."
```


**还使用 Context7 MCP 作为框架文档：**

对于计划中提到的任何技术/框架，请查询 Context7：
```
mcp__plugin_compound-engineering_context7__resolve-library-id: Find library ID for [framework]
mcp__plugin_compound-engineering_context7__query-docs: Query documentation for specific patterns
```


**使用 WebSearch 获取当前最佳实践：**

搜索有关计划中主题的最新 (2024-2025) 文章、博客文章和文档。

### 5.发现并运行所有审核代理

<thinking>
动态发现每个可用的代理并根据计划运行它们。不要过滤，不要跳过，不要假设相关性。 40+个并行代理就可以了。使用一切可用的东西。
</thinking>

**第 1 步：从所有来源发现所有可用代理**

```bash
# 1. Project-local agents (highest priority - project-specific)
find .claude/agents -name "*.md" 2>/dev/null

# 2. User's global agents (~/.claude/)
find ~/.claude/agents -name "*.md" 2>/dev/null

# 3. compound-engineering plugin agents (all subdirectories)
find ~/.claude/plugins/cache/*/compound-engineering/*/agents -name "*.md" 2>/dev/null

# 4. ALL other installed plugins - check every plugin for agents
find ~/.claude/plugins/cache -path "*/agents/*.md" 2>/dev/null

# 5. Check installed_plugins.json to find all plugin locations
cat ~/.claude/plugins/installed_plugins.json

# 6. For local plugins (isLocal: true), check their source directories
# Parse installed_plugins.json and find local plugin paths
```


**重要：**检查每个来源。包括来自以下机构的代理：
- 项目`.claude/agents/`
- 用户的`~/.claude/agents/`
- 复合工程插件（但跳过工作流程/代理 - 仅使用审查/、研究/、设计/、文档/）
- 所有其他已安装的插件（agent-sdk-dev、frontend-design 等）
- 任何本地插件

**特别针对复合工程插件：**
- 使用：`agents/review/*`（所有审阅者）
- 使用：`agents/research/*`（所有研究人员）
- 使用：`agents/design/*`（设计代理）
- 使用：`agents/docs/*`（文件代理）
- 跳过：`agents/workflow/*`（这些是工作流程协调者，而不是审阅者）

**第 2 步：对于每个发现的代理，请阅读其描述**

阅读每个代理文件的前几行以了解它审查/分析的内容。

**步骤 3：并行启动所有代理**

对于发现的每个代理，并行启动一个任务：

```
Task [agent-name]: "Review this plan using your expertise. Apply all your checks and patterns. Plan content: [full plan content]"
```


**关键规则：**
- 不要按“相关性”过滤代理 - 全部运行
- 不要跳过代理，因为他们“可能不申请” - 让他们决定
- 通过多个任务工具调用在一条消息中启动所有代理
- 20、30、40 个并行代理就可以了 - 使用一切
- 每个代理可能会发现其他代理错过的东西
- 目标是最大覆盖范围，而不是效率

**第 4 步：同时发现并运行研究代理**

研究代理（如`best-practices-researcher`、`framework-docs-researcher`、`git-history-analyzer`、`repo-research-analyst`）也应针对相关计划部分进行运行。

### 6. 等待所有代理并综合所有内容

<thinking>
等待所有并行代理完成 - 技能、研究代理、审查代理，一切。然后将所有发现综合为综合增强。
</thinking>

**从所有来源收集输出：**

1. **基于技能的子代理** - 每个技能的完整输出（代码示例、模式、建议）
2. **学习/解决方案子代理** - 来自/workflows:compound的相关记录学习
3. **研究代理** - 最佳实践、文档、真实示例
4. **审核代理** - 每个审核者的所有反馈（架构、安全性、性能、简单性等）
5. **Context7 查询** - 框架文档和模式
6. **网络搜索** - 当前最佳实践和文章

**对于每个代理的调查结果，摘录：**
- [ ] 具体建议（可操作的项目）
- [ ] 代码模式和示例（复制粘贴就绪）
- [ ] 要避免的反模式（警告）
- [ ] 性能考虑因素（指标、基准）
- [ ] 安全考虑（漏洞、缓解措施）
- [ ] 发现边缘情况（处理策略）
- [ ] 文档链接（参考文献）
- [ ] 技能特定模式（来自匹配的技能）
- [ ] 相关学习（过去适用的解决方案 - 防止重复错误）

**重复数据删除并确定优先级：**
- 合并来自多个代理的相似建议
- 按影响确定优先顺序（高价值改进优先）
- 标记相互矛盾的建议以供人工审核
- 按计划部分分组

### 7. 增强计划部分

<thinking>
将研究成果合并回计划中，在不改变原始结构的情况下增加深度。
</thinking>

**每个部分的增强格式：**

```markdown
## [Original Section Title]

[Original content preserved]

### Research Insights

**Best Practices:**
- [Concrete recommendation 1]
- [Concrete recommendation 2]

**Performance Considerations:**
- [Optimization opportunity]
- [Benchmark or metric to target]

**Implementation Details:**
```[language]

// 研究中的具体代码示例
```

**Edge Cases:**
- [Edge case 1 and how to handle]
- [Edge case 2 and how to handle]

**References:**
- [Documentation URL 1]
- [Documentation URL 2]
```


### 8.添加增强摘要

在计划的顶部添加摘要部分：

```markdown
## Enhancement Summary

**Deepened on:** [Date]
**Sections enhanced:** [Count]
**Research agents used:** [List]

### Key Improvements
1. [Major improvement 1]
2. [Major improvement 2]
3. [Major improvement 3]

### New Considerations Discovered
- [Important finding 1]
- [Important finding 2]
```


### 9. 更新计划文件

**写出增强计划：**
- 保留原始文件名
- 如果用户喜欢新文件，请添加 `-deepened` 后缀
- 更新任何时间戳或元数据

## 输出格式

更新计划文件（或根据要求创建`plans/<original-name>-deepened.md`）。

## 质量检查

在最终确定之前：
- [ ] 保留所有原始内容
- [ ] 明确标记和归因研究见解
- [ ] 代码示例语法正确
- [ ] 链接有效且相关
- [ ] 各部分之间没有矛盾
- [ ] 增强摘要准确反映变化

## 增强后选项

编写增强计划后，使用 **AskUserQuestion 工具** 呈现以下选项：

**问题：**“计划在`[plan_path]`深化。接下来你想做什么？”

**选项：**
1. **查看差异** - 显示添加/更改的内容
2. **运行 `/plan_review`** - 获取审阅者对增强计划的反馈
3. **开始`/workflows:work`** - 开始实施此增强计划
4. **进一步深化** - 对特定部分进行另一轮研究
5. **恢复** - 恢复原始计划（如果存在备份）

根据选择：
- **查看差异** → 运行`git diff [plan_path]`或显示之前/之后
- **`/plan_review`** → 使用计划文件路径调用/plan_review命令
- **`/workflows:work`** → 使用计划文件路径调用/workflows:work命令
- **进一步深化** → 询问哪些部分需要更多研究，然后重新运行这些代理
- **恢复** → 从 git 或备份恢复

## 增强示例

**之前（来自/workflows:plan）：**
```markdown
## Technical Approach

Use React Query for data fetching with optimistic updates.
```


**之后（来自/workflows:deepen-plan）：**
```markdown
## Technical Approach

Use React Query for data fetching with optimistic updates.

### Research Insights

**Best Practices:**
- Configure `staleTime` and `cacheTime` based on data freshness requirements
- Use `queryKey` factories for consistent cache invalidation
- Implement error boundaries around query-dependent components

**Performance Considerations:**
- Enable `refetchOnWindowFocus: false` for stable data to reduce unnecessary requests
- Use `select` option to transform and memoize data at query level
- Consider `placeholderData` for instant perceived loading

**Implementation Details:**
```typescript

// 推荐的查询配置
const queryClient = 新的 QueryClient({
  默认选项：{
    查询：{
      staleTime: 5 * 60 * 1000, // 5 分钟
      重试：2，
      refetchOnWindowFocus：假，
    },
  },
});
```

**Edge Cases:**
- Handle race conditions with `cancelQueries` on component unmount
- Implement retry logic for transient network failures
- Consider offline support with `persistQueryClient`

**References:**
- https://tanstack.com/query/latest/docs/react/guides/optimistic-updates
- https://tkdodo.eu/blog/practical-react-query
```


永远不要编码！只需研究并完善该计划即可。