---
name: compound-docs
description: 使用 YAML frontmatter 将已解决的问题捕获为分类文档，以便快速查找

allowed-tools:
  - Read # Parse conversation context
  - Write # Create resolution docs
  - Bash # Create directories
  - Grep # Search existing docs
preconditions:
  - Problem has been solved (not in-progress)
  - Solution has been verified working
---
# compound-docs 技能

**目的：** 自动记录已解决的问题，以通过基于类别的组织（枚举验证的问题类型）构建可搜索的机构知识。

## 概述

该技能在确认后立即捕获问题解决方案，创建结构化文档，作为未来会话的可搜索知识库。

**组织：** 单文件架构 - 每个问题都记录为其症状类别目录中的一个 markdown 文件（例如，`docs/solutions/performance-issues/n-plus-one-briefs.md`）。文件使用 YAML frontmatter 来实现元数据和可搜索性。

---

<critical_sequence name="documentation-capture" enforce_order="strict">

## 7 步流程

<step number="1" required="true">
### 第 1 步：检测确认

**在短语后自动调用：**

- “有效”
- “已经解决了”
- “现在工作”
-“问题已解决”
- “就是这样”

**或手册：** `/doc-fix`命令

**仅重要问题：**

- 需要多次调查尝试
- 棘手的调试需要时间
- 不明显的解决方案
- 未来的会议将会受益

**跳过以下文档：**

- 简单的错别字
- 明显的语法错误
- 琐碎的修复立即得到纠正
</step>

<step number="2" required="true" depends_on="1">
### 第 2 步：收集背景信息

对话历史记录摘录：

**所需信息：**

- **模块名称**：哪个 CORA 模块出现问题
- **症状**：可观察到的错误/行为（确切的错误消息）
- **调查尝试**：什么不起作用以及为什么不起作用
- **根本原因**：实际问题的技术解释
- **解决方案**：修复了什么（代码/配置更改）
- **预防**：将来如何避免

**环境详情：**

- 导轨版本
- 阶段（0-6 或实施后）
- 操作系统版本
- 文件/行参考

**阻止要求：** 如果缺少关键上下文（模块名称、确切错误、阶段或解决步骤），请在继续步骤 3 之前询问用户并等待响应：

```
I need a few details to document this properly:

1. Which module had this issue? [ModuleName]
2. What was the exact error message or symptom?
3. What stage were you in? (0-6 or post-implementation)

[Continue after user provides details]
```

</step>

<step number="3" required="false" depends_on="2">
### 步骤 3：检查现有文档

搜索 docs/solutions/ 查找类似问题：

```bash
# Search by error message keywords
grep -r "exact error phrase" docs/solutions/

# Search by symptom category
ls docs/solutions/[category]/
```


**如果发现类似问题：**

然后提出决策选项：

```
Found similar issue: docs/solutions/[path]

What's next?
1. Create new doc with cross-reference (recommended)
2. Update existing doc (only if same root cause)
3. Other

Choose (1-3): _
```


等待用户响应，然后执行所选操作。

**ELSE**（未发现类似问题）：

直接进入步骤 4（无需用户交互）。
</step>

<step number="4" required="true" depends_on="2">
### 步骤 4：生成文件名

格式：`[sanitized-symptom]-[module]-[YYYYMMDD].md`

**消毒规则：**

- 小写
- 用连字符替换空格
- 删除除连字符之外的特殊字符
- 截断至合理长度（< 80 chars)

**Examples:**

- `missing-include-BriefSystem-20251110.md`
- `parameter-not-saving-state-EmailProcessing-20251110.md`
- `webview-crash-on-resize-Assistant-20251110.md`
</step>

<step number="5" required="true" depends_on="4" blocking="true">
### 步骤 5：验证 YAML 架构

**关键：** 所有文档都需要经过验证的 YAML frontmatter 和枚举验证。

<validation_gate name="yaml-schema" blocking="true">

**根据架构进行验证：**
加载 `schema.yaml` 并根据 [yaml-schema.md](./references/yaml-schema.md) 中定义的枚举值对问题进行分类。确保所有必填字段都存在并且与允许的值完全匹配。

**如果验证失败则阻止：**

```
❌ YAML validation failed

Errors:
- problem_type: must be one of schema enums, got "compilation_error"
- severity: must be one of [critical, moderate, minor], got "high"
- symptoms: must be array with 1-5 items, got string

Please provide corrected values.
```


**控制执行：** 在 YAML frontmatter 通过 `schema.yaml` 中定义的所有验证规则之前，请勿继续执行步骤 6（创建文档）。

</validation_gate>
</step>

<step number="6" required="true" depends_on="5">
### 第 6 步：创建文档

**根据 Problem_type 确定类别：** 使用 [yaml-schema.md](./references/yaml-schema.md)（第 49-61 行）中定义的类别映射。

**创建文档文件：**

```bash
PROBLEM_TYPE="[from validated YAML]"
CATEGORY="[mapped from problem_type]"
FILENAME="[generated-filename].md"
DOC_PATH="docs/solutions/${CATEGORY}/${FILENAME}"

# Create directory if needed
mkdir -p "docs/solutions/${CATEGORY}"

# Write documentation using template from assets/resolution-template.md
# (Content populated with Step 2 context and validated YAML frontmatter)
```


**结果：**
- 类别目录中的单个文件
- 枚举验证确保分类一致

**创建文档：** 使用步骤 2 中收集的上下文和步骤 5 中验证的 YAML frontmatter 填充 `assets/resolution-template.md` 中的结构。
</step>

<step number="7" required="false" depends_on="6">
### 步骤 7：交叉引用和关键模式检测

如果在步骤3中发现类似问题：

**更新现有文档：**

```bash
# Add Related Issues link to similar doc
echo "- See also: [$FILENAME]($REAL_FILE)" >> [similar-doc.md]
```


**更新新文档：**
已包含步骤 6 中的交叉引用。

**更新模式（如果适用）：**

如果这代表了一种常见模式（3 个以上类似问题）：

```bash
# Add to docs/solutions/patterns/common-solutions.md
cat >> docs/solutions/patterns/common-solutions.md << 'EOF'

## [Pattern Name]

**Common symptom:** [Description]
**Root cause:** [Technical explanation]
**Solution pattern:** [General approach]

**Examples:**
- [Link to doc 1]
- [Link to doc 2]
- [Link to doc 3]
EOF
```


**关键模式检测（可选主动建议）：**

如果此问题有自动指示器表明该问题可能很严重：
- 严重性：YAML 中的 `critical`
- 影响多个模块或基础阶段（第 2 或第 3 阶段）
- 不明显的解决方案

然后在决策菜单（步骤 8）中添加注释：
```
💡 This might be worth adding to Required Reading (Option 2)
```


但**永远不要自动促销**。用户通过决策菜单做出决定（选项 2）。

**添加关键模式的模板：**

当用户选择选项 2（添加到必读内容）时，使用 `assets/critical-pattern-template.md` 中的模板来构建模式条目。根据 `docs/solutions/patterns/cora-critical-patterns.md` 中的现有模式对其进行顺序编号。
</step>

</critical_sequence>

---

<decision_gate name="post-documentation" wait_for_user="true">

## 捕获后的决策菜单

成功记录后，提供选项并等待用户响应：

```
✓ Solution documented

File created:
- docs/solutions/[category]/[filename].md

What's next?
1. Continue workflow (recommended)
2. Add to Required Reading - Promote to critical patterns (cora-critical-patterns.md)
3. Link related issues - Connect to similar problems
4. Add to existing skill - Add to a learning skill (e.g., hotwire-native)
5. Create new skill - Extract into new learning skill
6. View documentation - See what was captured
7. Other
```


**处理回复：**

**选项 1：继续工作流程**

- 返回呼叫技巧/工作流程
- 文档已完成

**选项 2：添加到必读** ⭐ 关键模式的主要路径

用户在以下情况下选择此选项：
- 系统在不同模块中多次犯此错误
- 解决方案并不明显，但每次都必须遵循
- 基础要求（Rails、Rails API、线程等）

行动：
1. 从文档中提取模式
2. 使用代码示例将格式设置为“❌错误”与“✅正确”
3. 添加至`docs/solutions/patterns/cora-critical-patterns.md`
4. 添加交叉引用回本文档
5. 确认：“✓ 添加到必读。所有子代理将在代码生成之前看到此模式。”

**选项 3：链接相关问题**

- 提示：“要链接哪个文档？（提供文件名或描述）”
- 在 docs/solutions/ 中搜索文档
- 添加对两个文档的交叉引用
- 确认：“✓ 添加了交叉引用”

**选项 4：添加到现有技能**

当记录的解决方案与现有学习技能相关时，用户选择此选项：

行动：
1. 提示：“哪项技能？（hotwire-native等）”
2. 确定要更新哪个参考文件（resources.md、patterns.md 或 Examples.md）
3. 在适当的部分添加链接和简要说明
4. 确认：“✓ 添加到[文件]中的[技能名称]技能”

示例：对于 Hotwire Native Tailwind 变体解决方案：
- 添加到“CORA 特定资源”下的 `hotwire-native/references/resources.md`
- 添加到 `hotwire-native/references/examples.md`，并包含解决方案文档的链接

**选项 5：创建新技能**

当解决方案代表新学习领域的开始时，用户选择此选项：

行动：
1. 提示：“新技能应该被称为什么？（例如，条带计费、电子邮件处理）”
2. 运行`python3 .claude/skills/skill-creator/scripts/init_skill.py [skill-name]`
3. 使用此解决方案作为第一个示例创建初始参考文件
4. 确认：“✓ 使用此解决方案作为第一个示例创建了新的 [技能名称] 技能”

**选项 6：查看文档**

- 显示创建的文档
- 再次呈现决策菜单

**选项 7：其他**

- 询问他们想做什么

</decision_gate>

---

<integration_protocol>

## 集成点

**调用者：**
- /compound命令（主接口）
- 解决方案确认后在对话中手动调用
- 可以通过检测“有效”、“已修复”等确认短语来触发。

**调用：**
- 无（终端技能 - 不委托给其他技能）

**交接期望：**
文档所需的所有上下文应在调用之前出现在对话历史记录中。

</integration_protocol>

---

<success_criteria>

## 成功标准

当满足以下所有条件时，文档记录成功：

- ✅ YAML frontmatter 已验证（所有必填字段，正确的格式）
- ✅ 在 docs/solutions/[category]/[filename].md 中创建的文件
- ✅ 枚举值与 schema.yaml 完全匹配
- ✅ 解决方案部分包含代码示例
- ✅ 如果发现相关问题，则添加交叉引用
- ✅ 向用户呈现决策菜单并确认操作

</success_criteria>

---

## 错误处理

**缺少上下文：**

- 询问用户缺少的详细信息
- 在提供关键信息之前不要继续

**YAML 验证失败：**

- 显示具体错误
- 使用更正值进行重试
- 封锁直到有效

**类似问题歧义：**

- 呈现多场比赛
- 让用户选择：新文档、更新现有文档或链接为重复文档

**模块不在 CORA-MODULES.md 中：**

- 警告但不要阻止
- 继续处理文档
- 建议：“将 [Module] 添加到 CORA-MODULES.md（如果不存在）”

---

## 执行指南

**必须做：**
- 验证 YAML frontmatter（如果按照第 5 步验证门无效则阻止）
- 从对话中提取准确的错误消息
- 在解决方案部分包含代码示例
- 在写入文件之前创建目录（`mkdir -p`）
- 如果缺少关键上下文，请询问用户并等待

**绝对不能做：**
- 跳过 YAML 验证（验证门被阻止）
- 使用模糊的描述（不可搜索）
- 省略代码示例或交叉引用

---

## 质量指南

**好的文档具有：**

- ✅ 准确的错误消息（从输出复制粘贴）
- ✅ 具体文件：行参考
- ✅ 可观察到的症状（您所看到的，而不是解释）
- 记录失败的尝试（有助于避免错误的路径）
- ✅ 技术解释（不仅仅是“什么”，还有“为什么”）
- ✅ 代码示例（之前/之后，如果适用）
- ✅预防指导（如何及早发现）
- ✅ 交叉引用（相关问题）

**避免：**

- ❌ 模糊的描述（“出了点问题”）
- ❌ 缺少技术细节（“修复代码”）
- ❌没有上下文（哪个版本？哪个文件？）
- ❌ 只是代码转储（解释它为什么有效）
- ❌ 无预防指导
- ❌ 无交叉引用

---

## 示例场景

**用户：**“成功了！N+1 查询已修复。”

**技能激活：**

1. **检测确认：**“有效！”触发自动调用
2. **收集上下文：**
   - 模块：简要系统
   - 症状：短暂生成需要 >5 秒，加载电子邮件线程时进行 N+1 查询
   - 失败的尝试：添加分页（没有帮助），检查后台作业性能
   - 解决方案：在 Brief 模型上添加了 `includes(:emails)` 的急切加载
   - 根本原因：缺少急切加载导致每个电子邮件线程单独进行数据库查询
3. **检查现有：** 未发现类似问题
4. **生成文件名：** `n-plus-one-brief-generation-BriefSystem-20251110.md`
5. **验证 YAML：**
   ```yaml
   module: Brief System
   date: 2025-11-10
   problem_type: performance_issue
   component: rails_model
   symptoms:
     - "N+1 query when loading email threads"
     - "Brief generation taking >5 seconds"
   root_cause: missing_include
   severity: high
   tags: [n-plus-one, eager-loading, performance]
   ```

   ✅ 有效
6. **创建文档：**
   - `docs/solutions/performance-issues/n-plus-one-brief-generation-BriefSystem-20251110.md`
7. **交叉引用：** 不需要（没有类似问题）

**输出：**

```
✓ Solution documented

File created:
- docs/solutions/performance-issues/n-plus-one-brief-generation-BriefSystem-20251110.md

What's next?
1. Continue workflow (recommended)
2. Add to Required Reading - Promote to critical patterns (cora-critical-patterns.md)
3. Link related issues - Connect to similar problems
4. Add to existing skill - Add to a learning skill (e.g., hotwire-native)
5. Create new skill - Extract into new learning skill
6. View documentation - See what was captured
7. Other
```


---

## 未来的增强

**不属于第 7 阶段范围，但有潜力：**

- 按日期范围搜索
- 按严重性过滤
- 基于标签的搜索界面
- 指标（最常见问题、解决时间）
- 导出为可共享格式（社区知识共享）
- 导入社区解决方案