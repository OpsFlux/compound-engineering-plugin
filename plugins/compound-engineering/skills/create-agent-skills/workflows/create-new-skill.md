# 工作流程：创建新技能

<required_reading>
**立即阅读这些参考文件：**
1.references/recommended-structure.md
2.references/skill-structure.md
3.references/core-principles.md
4.references/use-xml-tags.md
</required_reading>

<process>
## 步骤 1：适应性需求收集

**如果用户提供了上下文**（例如，“为 X 建立一项技能”）：
→ 分析所陈述的内容、可以推断的内容、不清楚的内容
→ 跳至仅询问真正的差距

**如果用户只是在没有上下文的情况下调用技能：**
→ 询问他们想要构建什么

### 使用 AskUserQuestion

根据实际差距提出 2-4 个特定领域的问题。每个问题应该：
- 有具体的选项和描述
- 关注范围、复杂性、产出、边界
- 不要问从上下文中显而易见的事情

示例问题：
——“这个技能具体应该处理什么操作？” （带有基于域的选项）
- “这也应该处理[相关的事情]还是继续专注于[核心的事情]？”
- “成功后用户应该看到什么？”

### 决策门

提出初步问题后，询问：
“准备好继续建造了，还是你想让我问更多问题？”

选项：
1. **继续构建** - 我有足够的背景信息
2. **提出更多问题** - 还有更多细节需要澄清
3. **让我添加详细信息** - 我想提供更多背景信息

## 步骤 2：研究触发器（如果是外部 API）

**当检测到外部服务**时，使用 AskUserQuestion 进行询问：
“这涉及[服务名称] API。您希望我在构建之前研究当前的端点和模式吗？”

选项：
1. **是的，首先进行研究** - 获取当前文档以准确实施
2. **不，继续使用通用模式** - 使用通用模式，无需进行特定的 API 研究

如果研究要求：
- 使用 Context7 MCP 获取当前库文档
- 或者使用 WebSearch 获取最新的 API 文档
- 关注2024-2025年来源
- 存储结果以用于内容生成

## 步骤 3：决定结构

**简单技巧（单一工作流程，<200行）：**
→ 包含所有内容的单个 SKILL.md 文件

**复杂技能（多个工作流程或领域知识）：**
→ 路由器模式：
```
skill-name/
├── SKILL.md (router + principles)
├── workflows/ (procedures - FOLLOW)
├── references/ (knowledge - READ)
├── templates/ (output structures - COPY + FILL)
└── scripts/ (reusable code - EXECUTE)
```


有利于路由器模式的因素：
- 多个不同的用户意图（创建、调试、发布）
- 跨工作流程共享领域知识
- 不可忽视的基本原则
- 技能可能会随着时间的推移而增长

**考虑模板/何时：**
- 技能产生一致的输出结构（计划、规格、报告）
- 结构比创意的产生更重要

**考虑脚本/何时：**
- 跨调用运行相同的代码（部署、设置、API 调用）
- 每次重写操作容易出错

有关模板，请参阅references/recommended-structure.md。

## 第四步：创建目录

```bash
mkdir -p ~/.claude/skills/{skill-name}
# If complex:
mkdir -p ~/.claude/skills/{skill-name}/workflows
mkdir -p ~/.claude/skills/{skill-name}/references
# If needed:
mkdir -p ~/.claude/skills/{skill-name}/templates  # for output structures
mkdir -p ~/.claude/skills/{skill-name}/scripts    # for reusable code
```


## 第五步：编写SKILL.md

**简单技能：** 使用以下内容编写完整的技能文件：
- YAML frontmatter（名称、描述）
- `<objective>`
- `<quick_start>`
- 纯 XML 内容部分
- `<success_criteria>`

**复杂的技巧：** 使用以下内容编写路由器：
- YAML 前言
- `<essential_principles>`（内联，不可避免）
- `<intake>`（询问用户的问题）
- `<routing>`（将答案映射到工作流程）
- `<reference_index>` 和`<workflows_index>`

## 步骤 6：编写工作流程（如果复杂）

对于每个工作流程：
```xml
<required_reading>
Which references to load for this workflow
</required_reading>

<process>
Step-by-step procedure
</process>

<success_criteria>
How to know this workflow is done
</success_criteria>
```


## 步骤 7：写参考文献（如果需要）

领域知识：
- 可能需要多个工作流程
- 不根据工作流程而改变
- 包含模式、示例、技术细节

## 步骤 8：验证结构

检查：
- [ ] YAML frontmatter 有效
- [ ] 名称与目录匹配（小写字母加连字符）
- [ ] 描述说明了它的作用以及何时使用它（第三人称）
- [ ] 正文中没有 Markdown 标题 (#) - 使用 XML 标签
- [ ] 必需的标签：objective、quick_start、success_criteria
- [ ] 所有引用的文件都存在
- [ ] SKILL.md 500行以下
- [ ] XML 标签正确关闭

## 步骤 9：创建斜杠命令

```bash
cat > ~/.claude/commands/{skill-name}.md << 'EOF'
---
description: {Brief description}
argument-hint: [{argument hint}]
allowed-tools: Skill({skill-name})
---

Invoke the {skill-name} skill for: $ARGUMENTS
EOF
```


## 第 10 步：测试

调用技能并观察：
- 它是否提出了正确的摄入问题？
- 它加载正确的工作流程吗？
- 工作流程是否加载正确的参考？
- 产出是否符合预期？

根据实际使用情况而不是假设进行迭代。
</process>

<success_criteria>
技能完成时：
- [ ] 通过适当的问题收集要求
- [ ] 如果涉及外部服务则完成 API 研究
- [ ] 目录结构正确
- [ ] SKILL.md 具有有效的 frontmatter
- [ ] 内联基本原则（如果技能复杂）
- [ ] 将问题引入正确的工作流程
- [ ] 所有工作流程都有 required_reading + process + success_criteria
- [ ] 参考文献包含可重用的领域知识
- [ ] 斜杠命令存在并且有效
- [ ] 通过真实调用进行测试
</success_criteria>