# 工作流程：添加对现有技能的引用

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


出示编号列表，问：“哪项技能需要新参考？”

## 步骤 2：分析当前结构

```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
ls ~/.claude/skills/{skill-name}/references/ 2>/dev/null
```


确定：
- **有references/文件夹吗？** → 好，可以直接添加
- **简单技巧？** → 可能需要先创建引用/
- **存在哪些参考文献？** → 了解知识格局

向用户报告当前引用。

## 步骤 3：收集参考要求

问：
- 该参考文献应包含哪些知识？
- 哪些工作流程将使用它？
- 这可以跨工作流程重复使用还是特定于一个工作流程？

**如果特定于一个工作流程** → 考虑将其内联到该工作流程中。

## 步骤 4：创建参考文件

创建`references/{reference-name}.md`：

使用语义 XML 标签来构建内容：
```xml
<overview>
Brief description of what this reference covers
</overview>

<patterns>
## Common Patterns
[Reusable patterns, examples, code snippets]
</patterns>

<guidelines>
## Guidelines
[Best practices, rules, constraints]
</guidelines>

<examples>
## Examples
[Concrete examples with explanation]
</examples>
```


## 步骤5：更新SKILL.md

添加对 `<reference_index>` 的新引用：
```markdown
**Category:** existing.md, new-reference.md
```


## 步骤 6：更新需要的工作流程

对于应使用此参考的每个工作流程：

1. 读取工作流程文件
2. 添加到其`<required_reading>`部分
3. 验证此添加后工作流程仍然有意义

## 第 7 步：验证

- [ ] 参考文件存在并且结构良好
- [ ] 参考位于 SKILL.md reference_index 中
- [ ] 相关工作流程已在 required_reading 中
- [ ] 没有损坏的参考
</process>

<success_criteria>
参考添加在以下情况下完成：
- [ ] 使用有用内容创建的参考文件
- [ ] 添加到 SKILL.md 中的reference_index
- [ ] 相关工作流程更新以供阅读
- [ ] 内容可重复使用（不特定于工作流程）
</success_criteria>