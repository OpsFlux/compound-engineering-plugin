# 工作流程：审核技能

<required_reading>
**立即阅读这些参考文件：**
1.references/recommended-structure.md
2.references/skill-structure.md
3.references/use-xml-tags.md
</required_reading>

<process>
## 第 1 步：列出可用技能

**不要使用 AskUserQuestion** - 可能有很多技巧。

将聊天中的技能枚举为编号列表：
```bash
ls ~/.claude/skills/
```


呈现为：
```
Available skills:
1. create-agent-skills
2. build-macos-apps
3. manage-stripe
...
```


问：“您想审核哪项技能？（输入号码或姓名）”

## 第 2 步：阅读技能

用户选择后，阅读完整的技能结构：
```bash
# Read main file
cat ~/.claude/skills/{skill-name}/SKILL.md

# Check for workflows and references
ls ~/.claude/skills/{skill-name}/
ls ~/.claude/skills/{skill-name}/workflows/ 2>/dev/null
ls ~/.claude/skills/{skill-name}/references/ 2>/dev/null
```


## 步骤 3：运行审核清单

根据每个标准进行评估：

### YAML Frontmatter
- [ ] 具有 `name:` 字段（小写字母带连字符）
- [ ] 名称与目录名称匹配
- [ ] 有 `description:` 字段
- [ ] 描述说明它的作用以及何时使用它
- [ ] 描述为第三人称（“在……时使用”）

### 结构
- [ ] SKILL.md 500行以下
- [ ] 纯 XML 结构（正文中没有 markdown 标题#）
- [ ] 所有 XML 标签正确关闭
- [ ] 具有必需的标签：objective 或 Essential_principles
- [ ] 有 success_criteria

### 路由器模式（如果技能复杂）
- [ ] SKILL.md 中内联的基本原则（不在单独的文件中）
- [ ] 有摄入问题
- [ ] 有路由表
- [ ] 所有引用的工作流程文件均存在
- [ ] 所有引用的参考文件均存在

### 工作流程（如果存在）
- [ ] 每个都有 required_reading 部分
- [ ] 每个都有进程部分
- [ ] 每个都有 success_criteria 部分
- [ ] 存在必需的阅读参考文献

### 内容质量
- [ ] 原则是可操作的（不是含糊的陈词滥调）
- [ ] 步骤是具体的（不是“做事”）
- [ ] 成功标准是可验证的
- [ ] 跨文件没有冗余内容

## 步骤 4：生成报告

目前的调查结果如下：

```
## Audit Report: {skill-name}

### ✅ Passing
- [list passing items]

### ⚠️ Issues Found
1. **[Issue name]**: [Description]
   → Fix: [Specific action]

2. **[Issue name]**: [Description]
   → Fix: [Specific action]

### 📊 Score: X/Y criteria passing
```


## 步骤 5：提供修复

如果发现问题，请询问：
“你想让我解决这些问题吗？”

选项：
1. **全部修复** - 应用所有建议的修复
2. **逐一修复** - 在应用之前查看每个修复
3. **仅报告** - 无需更改

如果修复：
- 做出每一个改变
- 每次更改后验证文件有效性
- 报告已修复的内容
</process>

<audit_anti_patterns>
## 要标记的常见反模式

**可跳过的原则**：基本原则在单独的文件中而不是内联
**单体技能**：单个文件超过500行
**混合关注点**：同一文件中的程序和知识
**含糊的步骤**：“适当处理错误”
**无法测试的标准**：“用户满意”
**正文中的 Markdown 标题**：使用 # 代替 XML 标签
**缺少路由**：没有摄入/路由的复杂技能
**损坏的引用**：提到但不存在的文件
**冗余内容**：多处相同信息
</audit_anti_patterns>

<success_criteria>
审核在以下情况下完成：
- [ ]技能全面阅读和分析
- [ ] 评估所有清单项目
- [ ] 向用户呈现的报告
- [ ] 应用修复（如果需要）
- [ ] 用户清楚了解技能健康状况
</success_criteria>