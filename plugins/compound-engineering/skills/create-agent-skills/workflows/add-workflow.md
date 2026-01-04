# 工作流程：向现有技能添加工作流程

<required_reading>
**立即阅读这些参考文件：**
1.references/recommended-structure.md
2.references/workflows-and-validation.md
</required_reading>

<process>
## 第 1 步：选择技能

**不要使用 AskUserQuestion** - 可能有很多技巧。

```bash
ls ~/.claude/skills/
```


出示编号列表，询问：“哪种技能需要新的工作流程？”

## 步骤 2：分析当前结构

阅读技能：
```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
ls ~/.claude/skills/{skill-name}/workflows/ 2>/dev/null
```


确定：
- **简单技巧？** → 可能需要先升级到路由器模式
- **已有工作流程/?** → 好，可以直接添加
- **存在哪些工作流程？** → 避免重复

向用户报告当前结构。

## 步骤 3：收集工作流程要求

使用 AskUserQuestion 或直接问题进行提问：
- 这个工作流程应该做什么？
- 与现有工作流程相比，什么时候有人会使用它？
- 需要什么参考资料？

## 步骤 4：升级到路由器模式（如果需要）

**如果目前技能很简单（没有工作流程/）：**

问：“这个技能需要先升级为路由器纹，需要重构吗？”

如果是：
1.创建workflows/目录
2. 将现有流程内容移至workflows/main.md
3.将SKILL.md重写为router，入口+路由
4. 在继续之前验证结构是否有效

## 步骤 5：创建工作流程文件

创建`workflows/{workflow-name}.md`：

```markdown
# Workflow: {Workflow Name}

<required_reading>
**Read these reference files NOW:**
1. references/{relevant-file}.md
</required_reading>

<process>
## Step 1: {First Step}
[What to do]

## Step 2: {Second Step}
[What to do]

## Step 3: {Third Step}
[What to do]
</process>

<success_criteria>
This workflow is complete when:
- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3
</success_criteria>
```


## 第 6 步：更新 SKILL.md

将新工作流程添加到：

1. **引入问题** - 添加新选项
2. **路由表** - 将选项映射到工作流程文件
3. **工作流程索引** - 添加到列表

## 步骤 7：创建参考（如果需要）

如果工作流程需要不存在的领域知识：
1. 创建`references/{reference-name}.md`
2. 在SKILL.md中添加reference_index
3.在工作流程的required_reading中引用它

## 步骤 8：测试

调用技能：
- 新选项是否出现在摄入量中？
- 选择它是否会通往正确的工作流程？
- 工作流程是否加载正确的参考？
- 工作流程是否正确执行？

向用户报告结果。
</process>

<success_criteria>
工作流程添加在以下情况下完成：
- [ ]技能升级为路由器模式（如果需要）
- [ ] 使用 required_reading、process、success_criteria 创建的工作流程文件
- [ ] SKILL.md 摄入量更新为新选项
- [ ] SKILL.md 路由更新
- [ ] SKILL.mdworkflows_index 已更新
- [ ] 创建任何需要的参考
- [ ] 已测试并正常工作
</success_criteria>