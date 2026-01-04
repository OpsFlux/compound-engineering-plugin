---
name: workflows:compound
description: 记录最近解决的问题以丰富团队的知识

argument-hint: "[optional: brief context about the fix]"
---

# /compound

协调并行工作的多个子代理以记录最近解决的问题。

## 目的

在上下文新鲜时捕获问题解决方案，使用 YAML frontmatter 在 `docs/solutions/` 中创建结构化文档，以供搜索和将来参考。使用并行子代理以获得最大效率。

**为什么要“复合”？** 每个记录在案的解决方案都会复合您团队的知识。第一次解决问题需要进行研究。记录下来，下一次发生需要几分钟的时间。知识复合。

## 用法

```bash
/workflows:compound                    # Document the most recent fix
/workflows:compound [brief context]    # Provide additional context hint
```


## 执行策略：并行子代理

此命令并行启动多个专用子代理以最大限度地提高效率：

### 1. **上下文分析器**（并行）
   - 提取对话历史记录
   - 识别问题类型、组成部分、症状
   - 根据 CORA 模式进行验证
   - 返回：YAML frontmatter 骨架

### 2. **解决方案提取器**（并行）
   - 分析所有调查步骤
   - 找出根本原因
   - 通过代码示例提取工作解决方案
   - 返回：解决方案内容块

### 3. **相关文档查找器**（并行）
   - 搜索`docs/solutions/`相关文档
   - 识别交叉引用和链接
   - 查找相关的 GitHub 问题
   - 返回：链接和关系

### 4. **预防策略师**（平行）
   - 制定预防策略
   - 创建最佳实践指南
   - 生成测试用例（如果适用）
   - 退货：预防/测试内容

### 5. **类别分类器**（并行）
   - 确定最佳`docs/solutions/`类别
   - 根据模式验证类别
   - 根据 slug 建议文件名
   - 返回：最终路径和文件名

### 6. **文档编写者**（并行）
   - 组装完整的 Markdown 文件
   - 验证 YAML frontmatter
   - 格式化内容以提高可读性
   - 在正确的位置创建文件

### 7. **可选：专业代理调用**（文档后）
   根据检测到的问题类型，自动调用适用的代理：
   - **性能问题** → `performance-oracle`
   - **安全问题** → `security-sentinel`
   - **数据库问题** → `data-integrity-guardian`
   - **测试失败** → `cora-test-reviewer`
   - 任何代码密集型问题 → `kieran-rails-reviewer` + `code-simplicity-reviewer`

## 它捕获了什么

- **问题症状**：确切的错误消息，可观察的行为
- **尝试的调查步骤**：什么不起作用以及原因
- **根本原因分析**：技术解释
- **工作解决方案**：使用代码示例逐步修复
- **预防策略**：未来如何避免
- **交叉引用**：相关问题和文档的链接

## 前提条件

<preconditions enforcement="advisory">
  <check condition="problem_solved">
    问题已解决（未进行中）
  </check>
  <check condition="solution_verified">
    解决方案已被验证有效
  </check>
  <check condition="non_trivial">
    不平凡的问题（不是简单的拼写错误或明显的错误）
  </check>
</preconditions>

## 它创造了什么

**组织文档：**

- 文件：`docs/solutions/[category]/[filename].md`

**从问题中自动检测的类别：**

- 构建错误/
- 测试失败/
- 运行时错误/
- 性能问题/
- 数据库问题/
- 安全问题/
- 用户界面错误/
- 整合问题/
- 逻辑错误/

## 成功输出

```
✓ Parallel documentation generation complete

Primary Subagent Results:
  ✓ Context Analyzer: Identified performance_issue in brief_system
  ✓ Solution Extractor: Extracted 3 code fixes
  ✓ Related Docs Finder: Found 2 related issues
  ✓ Prevention Strategist: Generated test cases
  ✓ Category Classifier: docs/solutions/performance-issues/
  ✓ Documentation Writer: Created complete markdown

Specialized Agent Reviews (Auto-Triggered):
  ✓ performance-oracle: Validated query optimization approach
  ✓ kieran-rails-reviewer: Code examples meet Rails standards
  ✓ code-simplicity-reviewer: Solution is appropriately minimal
  ✓ every-style-editor: Documentation style verified

File created:
- docs/solutions/performance-issues/n-plus-one-brief-generation.md

This documentation will be searchable for future reference when similar
issues occur in the Email Processing or Brief System modules.

What's next?
1. Continue workflow (recommended)
2. Link related documentation
3. Update other references
4. View documentation
5. Other
```


## 复合哲学

这就创建了一个复合知识系统：

1. 第一次解决“简短生成中的 N+1 查询” → 研究（30 分钟）
2. 记录解决方案 → docs/solutions/performance-issues/n-plus-one-briefs.md（5 分钟）
3.下次出现类似问题→快速查找（2分钟）
4. 知识复合→团队变得更聪明

反馈循环：

```
Build → Test → Find Issue → Research → Improve → Document → Validate → Deploy
    ↑                                                                      ↓
    └──────────────────────────────────────────────────────────────────────┘
```


**每个工程工作单元都应该使后续工作单元变得更容易，而不是更困难。**

## 自动调用

<auto_invoke> <trigger_phrases> - “有效” - “已修复” - “现在工作” - “问题已解决” </trigger_phrases>

<manual_override> 使用/workflows:compound [上下文] 立即记录，无需等待自动检测。 </manual_override></auto_invoke>

## 前往的路线

`compound-docs`技能

## 适用的专业代理商

根据问题类型，这些代理可以增强文档记录：

### 代码质量和审查
- **kieran-rails-reviewer**：审查 Rails 最佳实践的代码示例
- **code-simplicity-reviewer**：确保解决方案代码最少且清晰
- **模式识别专家**：识别反模式或重复问题

### 特定领域专家
- **performance-oracle**：分析 Performance_issue 类别解决方案
- **security-sentinel**：审查安全问题解决方案中的漏洞
- **cora-test-reviewer**：为预防策略创建测试用例
- **data-integrity-guardian**：审查数据库问题迁移和查询

### 增强和文档
- **最佳实践研究员**：通过行业最佳实践丰富解决方案
- **every-style-editor**：审查文档风格和清晰度
- **framework-docs-researcher**：Rails/gem 文档参考的链接

### 何时调用
- **自动触发**（可选）：代理可以运行后期文档以进行增强
- **手动触发**：用户可以在/workflows:compound完成后调用代理以进行更深入的审查

## 相关命令

- `/research [topic]` - 深入调查（搜索文档/解决方案/模式）
- `/workflows:plan` - 规划工作流程（参考记录的解决方案）