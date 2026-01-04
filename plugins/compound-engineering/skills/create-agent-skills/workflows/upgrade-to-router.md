# 工作流程：将技能升级到路由器模式

<required_reading>
**立即阅读这些参考文件：**
1.references/recommended-structure.md
2.references/skill-structure.md
</required_reading>

<process>
## 第 1 步：选择技能

```bash
ls ~/.claude/skills/
```


出示编号表，问：“哪项技能应该升级到路由器模式？”

## 步骤 2：验证是否需要升级

阅读技能：
```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
ls ~/.claude/skills/{skill-name}/
```


**已经是路由器？**（有工作流程/和摄入问题）
→ 告诉用户它已经在使用路由器模式，建议改为添加工作流程

**简单的技能应该保持简单？**（200 行以下，单一工作流程）
→ 解释路由器模式可能有点过分，询问他们是否仍想继续

**升级的良好候选者：**
- 超过200行
- 多个不同的用例
- 不应忽略的基本原则
- 日益复杂

## 步骤 3：识别组件

分析当前技能并确定：

1. **基本原则** - 适用于所有用例的规则
2. **不同的工作流程** - 用户可能想做的不同事情
3. **可重用的知识** - 模式、示例、技术细节

目前的发现：
```
## Analysis

**Essential principles I found:**
- [Principle 1]
- [Principle 2]

**Distinct workflows I identified:**
- [Workflow A]: [description]
- [Workflow B]: [description]

**Knowledge that could be references:**
- [Reference topic 1]
- [Reference topic 2]
```


问：“这个细分看起来正确吗？有什么调整吗？”

## 步骤 4：创建目录结构

```bash
mkdir -p ~/.claude/skills/{skill-name}/workflows
mkdir -p ~/.claude/skills/{skill-name}/references
```


## 步骤 5：提取工作流程

对于每个已确定的工作流程：

1. 创建`workflows/{workflow-name}.md`
2.添加required_reading部分（需要的引用）
3.添加流程部分（原技能步骤）
4. 添加 success_criteria 部分

## 步骤 6：提取参考文献

对于每个确定的参考主题：

1. 创建`references/{reference-name}.md`
2.从原有技能中移出相关内容
3. 具有语义 XML 标签的结构

## 步骤7：将SKILL.md重写为Router

将 SKILL.md 替换为路由器结构：

```markdown
---
name: {skill-name}
description: {existing description}
---

<essential_principles>
[Extracted principles - inline, cannot be skipped]
</essential_principles>

<intake>
**Ask the user:**

What would you like to do?
1. [Workflow A option]
2. [Workflow B option]
...

**Wait for response before proceeding.**
</intake>

<routing>
| Response | Workflow |
|----------|----------|
| 1, "keywords" | `workflows/workflow-a.md` |
| 2, "keywords" | `workflows/workflow-b.md` |
</routing>

<reference_index>
[List all references by category]
</reference_index>

<workflows_index>
| Workflow | Purpose |
|----------|---------|
| workflow-a.md | [What it does] |
| workflow-b.md | [What it does] |
</workflows_index>
```


## 步骤 8：确认没有丢失任何东西

将原始技能内容与新结构进行比较：
- [ ] 保留所有原则（现在内联）
- [ ] 保留所有程序（现在在工作流程中）
- [ ] 保留所有知识（现在在参考文献中）
- [ ] 没有孤立内容

## 步骤 9：测试

调用升级后的技能：
- 是否出现摄入问题？
- 每个路由选项都有效吗？
- 工作流程加载正确的参考吗？
- 行为与原始技能相符吗？

报告任何问题。
</process>

<success_criteria>
升级完成时：
- [ ] 使用工作流程文件创建的工作流程/目录
- 创建 [ ] 引用/目录（如果需要）
- [ ] SKILL.md 重写为路由器
- [ ] SKILL.md 中内嵌的基本原则
- [ ] 保留所有原始内容
- [ ] 正确接收问题路线
- [ ] 已测试并正常工作
</success_criteria>