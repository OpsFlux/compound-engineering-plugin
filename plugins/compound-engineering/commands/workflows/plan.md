---
name: workflows:plan
description: 按照约定将功能描述转换为结构良好的项目计划

argument-hint: "[feature description, bug report, or improvement idea]"
---
# 为新功能或错误修复制定计划

## 介绍

**注意：当前年份是 2026 年。** 在约会计划和搜索最近的文档时使用此选项。

将功能描述、错误报告或改进想法转换为遵循项目约定和最佳实践的结构良好的 Markdown 文件问题。此命令提供灵活的详细级别以满足您的需求。

## 功能描述

<feature_description> #$参数</feature_description>

**如果上面的功能描述为空，请询问用户：**“您想要计划什么？请描述您想要的功能、错误修复或改进。”

在用户提供清晰的功能描述之前，请勿继续。

## 主要任务

### 1. 存储库研究和背景收集

<thinking>
首先，我需要了解项目的惯例和现有模式，利用所有可用资源并使用并行子代理来完成此任务。
</thinking>

同时并行运行这三个代理：

- 任务repo-research-analyst(feature_description)
- 任务最佳实践研究员（功能描述）
- 任务框架-docs-researcher(feature_description)

**参考资料集：**

- [ ] 使用特定文件路径记录所有研究结果（例如，`app/services/example_service.rb:42`）
- [ ] 包含外部文档和最佳实践指南的 URL
- [ ] 创建类似问题或 PR 的参考列表（例如，`#123`、`#456`）
- [ ] 注意 `CLAUDE.md` 或团队文档中发现的任何团队约定

### 2. 发行计划和结构

<thinking>
像产品经理一样思考 - 怎样才能使这个问题变得清晰且可操作？考虑多种观点
</thinking>

**标题和分类：**

- [ ] 使用传统格式起草清晰、可搜索的问题标题（例如，`feat:`、`fix:`、`docs:`）
- [ ] 确定问题类型：增强、错误、重构

**利益相关者分析：**

- [ ] 确定谁将受到此问题的影响（最终用户、开发人员、运营人员）
- [ ] 考虑实施复杂性和所需的专业知识

**内容策划：**

- [ ] 根据问题复杂性和受众选择适当的详细程度
- [ ] 列出所选模板的所有必要部分
- [ ] 收集支持材料（错误日志、屏幕截图、设计模型）
- [ ] 准备代码示例或复制步骤（如果适用），在列表中命名模拟文件名

### 3. SpecFlow 分析

规划问题结构后，运行 SpecFlow Analyzer 来验证和细化功能规范：

- 任务规格流分析器（特征描述，研究结果）

**SpecFlow 分析仪输出：**

- [ ] 查看 SpecFlow 分析结果
- [ ] 将任何已识别的差距或边缘情况纳入问题中
- [ ] 根据 SpecFlow 发现更新验收标准

### 4. 选择实施细节级别

选择您希望问题的全面程度，越简单越好。

#### 📄 极简（快速发行）

**最适合：** 简单的错误、小的改进、清晰的功能

**包括：**

- 问题陈述或功能描述
- 基本验收标准
- 仅重要上下文

**结构：**

````markdown
[Brief problem/feature description]

## Acceptance Criteria

- [ ] Core requirement 1
- [ ] Core requirement 2

## Context

[Any critical information]

## MVP

### test.rb

```ruby
class Test
  def initialize
    @name = "test"
  end
end
```

## References

- Related issue: #[issue_number]
- Documentation: [relevant_docs_url]
````


#### 📋 更多（标准版）

**最适合：** 大多数功能、复杂的错误、团队协作

**包括 MINIMAL plus 的所有内容：**

- 详细的背景和动机
- 技术考虑
- 成功指标
- 依赖性和风险
- 基本实施建议

**结构：**

```markdown
## Overview

[Comprehensive description]

## Problem Statement / Motivation

[Why this matters]

## Proposed Solution

[High-level approach]

## Technical Considerations

- Architecture impacts
- Performance implications
- Security considerations

## Acceptance Criteria

- [ ] Detailed requirement 1
- [ ] Detailed requirement 2
- [ ] Testing requirements

## Success Metrics

[How we measure success]

## Dependencies & Risks

[What could block or complicate this]

## References & Research

- Similar implementations: [file_path:line_number]
- Best practices: [documentation_url]
- Related PRs: #[pr_number]
```


#### 📚 很多（综合问题）

**最适合：** 主要功能、架构变化、复杂集成

**包括 MORE 的所有内容以及：**

- 分阶段的详细实施计划
- 考虑替代方法
- 广泛的技术规格
- 资源要求和时间表
- 未来的考虑和可扩展性
- 风险缓解策略
- 文件要求

**结构：**

```markdown
## Overview

[Executive summary]

## Problem Statement

[Detailed problem analysis]

## Proposed Solution

[Comprehensive solution design]

## Technical Approach

### Architecture

[Detailed technical design]

### Implementation Phases

#### Phase 1: [Foundation]

- Tasks and deliverables
- Success criteria
- Estimated effort

#### Phase 2: [Core Implementation]

- Tasks and deliverables
- Success criteria
- Estimated effort

#### Phase 3: [Polish & Optimization]

- Tasks and deliverables
- Success criteria
- Estimated effort

## Alternative Approaches Considered

[Other solutions evaluated and why rejected]

## Acceptance Criteria

### Functional Requirements

- [ ] Detailed functional criteria

### Non-Functional Requirements

- [ ] Performance targets
- [ ] Security requirements
- [ ] Accessibility standards

### Quality Gates

- [ ] Test coverage requirements
- [ ] Documentation completeness
- [ ] Code review approval

## Success Metrics

[Detailed KPIs and measurement methods]

## Dependencies & Prerequisites

[Detailed dependency analysis]

## Risk Analysis & Mitigation

[Comprehensive risk assessment]

## Resource Requirements

[Team, time, infrastructure needs]

## Future Considerations

[Extensibility and long-term vision]

## Documentation Plan

[What docs need updating]

## References & Research

### Internal References

- Architecture decisions: [file_path:line_number]
- Similar features: [file_path:line_number]
- Configuration: [file_path:line_number]

### External References

- Framework documentation: [url]
- Best practices guide: [url]
- Industry standards: [url]

### Related Work

- Previous PRs: #[pr_numbers]
- Related issues: #[issue_numbers]
- Design documents: [links]
```


### 5.问题创建和格式设置

<thinking>
应用最佳实践以提高清晰度和可操作性，使问题易于扫描和理解
</thinking>

**内容格式：**

- [ ] 使用清晰的描述性标题和适当的层次结构（##、###）
- [ ] 在三个反引号中包含代码示例，并突出显示语言语法
- [ ] 添加与 UI 相关的屏幕截图/模型（拖放或使用图像托管）
- [ ] 使用任务列表 (- [ ]) 来查看可勾选的可跟踪项目
- [ ] 使用 `<details>` 标签为冗长的日志或可选细节添加可折叠部分
- [ ] 应用适当的表情符号进行视觉扫描（🐛 bug、✨ 功能、📚 文档、♻️ 重构）

**交叉参考：**

- [ ] 使用#number 格式链接到相关问题/PR
- [ ] 在相关时使用 SHA 哈希引用特定提交
- [ ] 使用 GitHub 的永久链接功能链接到代码（按“y”获取永久链接）
- [ ] 如果需要，使用@用户名提及相关团队成员
- [ ] 添加带有描述性文本的外部资源链接

**代码和示例：**

````markdown
# Good example with syntax highlighting and line references


```ruby
# app/services/user_service.rb:42
def process_user(user)

# Implementation here

end
```

# Collapsible error logs

<details>
<summary>Full error stacktrace</summary>

`Error details here...`

</details>
````


**人工智能时代的考虑因素：**

- [ ] 通过 AI 结对编程加速开发
- [ ] 包括在研究过程中效果良好的提示或说明
- [ ] 注意哪些人工智能工具用于初始探索（Claude、Copilot 等）
- [ ] 考虑到快速实施，强调全面测试
- [ ] 记录任何需要人工审核的人工智能生成的代码

### 6. 最终审核和提交

**预提交清单：**

- [ ] 标题可搜索且具有描述性
- [ ] 标签准确地对问题进行分类
- [ ] 所有模板部分均已完成
- [ ] 链接和参考有效
- [ ] 验收标准是可衡量的
- [ ] 在伪代码示例和待办事项列表中添加文件名
- [ ] 添加 ERD 美人鱼图（如果适用于新模型更改）

## 输出格式

将计划写入`plans/<issue_title>.md`

## 后生成选项

编写计划文件后，使用 **AskUserQuestion 工具** 呈现以下选项：

**问题：**“计划已在`plans/<issue_title>.md`准备好。接下来你想做什么？”

**选项：**
1. **在编辑器中打开计划** - 打开计划文件进行审查
2. **运行`/deepen-plan`** - 使用并行研究代理增强每个部分（最佳实践、性能、UI）
3. **运行 `/plan_review`** - 从审稿人那里获取反馈（DHH、Kieran、Simplicity）
4. **开始`/workflows:work`** - 开始在当地实施该计划
5. **在远程启动`/workflows:work`** - 开始在网络上的 Claude Code 中实施（使用`&` 在后台运行）
6. **创建问题** - 在项目跟踪器中创建问题 (GitHub/Linear)
7. **简化** - 降低细节级别

根据选择：
- **在编辑器中打开计划** → 运行`open plans/<issue_title>.md`以在用户的默认编辑器中打开文件
- **`/deepen-plan`** → 使用计划文件路径调用/deepen-plan命令以通过研究进行增强
- **`/plan_review`** → 使用计划文件路径调用/plan_review命令
- **`/workflows:work`** → 使用计划文件路径调用/workflows:work命令
- **`/workflows:work` 远程** → 运行`/workflows:work plans/<issue_title>.md &` 开始在后台为 Claude Code web 工作
- **创建问题** → 请参阅下面的“问题创建”部分
- **简化** → 问“我应该简化什么？”然后重新生成更简单的版本
- **其他**（自动提供）→ 接受自由文本进行返工或特定更改

**注意：** 如果在启用 ultrathink 的情况下运行`/workflows:plan`，则在创建计划后自动运行`/deepen-plan`以获得最大深度和基础。

在简化或其他更改后循环返回选项，直到用户选择`/workflows:work` 或`/plan_review`。

## 问题创建

当用户选择“创建问题”时，从 CLAUDE.md 检测他们的项目跟踪器：

1. **检查用户的 CLAUDE.md 中的跟踪器首选项**（全局或项目）：
   - 寻找`project_tracker: github`或`project_tracker: linear`
   - 或者在其工作流程部分查找“GitHub 问题”或“线性”的提及

2. **如果是 GitHub：**
   ```bash
   # Extract title from plan filename (kebab-case to Title Case)
   # Read plan content for body
   gh issue create --title "feat: [Plan Title]" --body-file plans/<issue_title>.md
   ```


3. **如果是线性的：**
   ```bash
   # Use linear CLI if available, or provide instructions
   # linear issue create --title "[Plan Title]" --description "$(cat plans/<issue_title>.md)"
   ```


4. **如果没有配置跟踪器：**
   询问用户：“您使用哪个项目跟踪器？(GitHub/Linear/Other)”
   - 建议将 `project_tracker: github` 或 `project_tracker: linear` 添加到他们的 CLAUDE.md

5. **创建后：**
   - 显示问题 URL
   - 询问他们是否想继续进行 `/workflows:work` 或 `/plan_review`

永远不要编码！只需研究并编写计划即可。