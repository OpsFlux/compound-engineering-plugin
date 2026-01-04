# 在技能中使用模板

<purpose>
模板是 Claude 复制和填写的可重复使用的输出结构。它们确保一致、高质量的输出，而无需每次都重新生成结构。
</purpose>

<when_to_use>
在以下情况下使用模板：
- 跨调用的输出应该具有一致的结构
- 结构比创意的产生更重要
- 填充占位符比生成空白页更可靠
- 用户期望可预测的、专业的输出

常见模板类型：
- **计划** - 项目计划、实施计划、迁移计划
- **规格** - 技术规格、功能规格、API 规格
- **文件** - 报告、提案、摘要
- **配置** - 配置文件、设置、环境设置
- **脚手架** - 文件结构、样板代码
</when_to_use>

<template_structure>
模板位于技能目录中的`templates/`中：

```
skill-name/
├── SKILL.md
├── workflows/
├── references/
└── templates/
    ├── plan-template.md
    ├── spec-template.md
    └── report-template.md
```


模板文件包含：
1. 清除截面标记
2. 占位符指示符（使用`{{placeholder}}`或`[PLACEHOLDER]`）
3. 内联指导，指示什么去哪里
4. 有帮助的示例内容
</template_structure>

<template_example>
```markdown
# {{PROJECT_NAME}} Implementation Plan

## Overview
{{1-2 sentence summary of what this plan covers}}

## Goals
- {{Primary goal}}
- {{Secondary goals...}}

## Scope
**In scope:**
- {{What's included}}

**Out of scope:**
- {{What's explicitly excluded}}

## Phases

### Phase 1: {{Phase name}}
**Duration:** {{Estimated duration}}
**Deliverables:**
- {{Deliverable 1}}
- {{Deliverable 2}}

### Phase 2: {{Phase name}}
...

## Success Criteria
- [ ] {{Measurable criterion 1}}
- [ ] {{Measurable criterion 2}}

## Risks
| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| {{Risk}} | {{H/M/L}} | {{H/M/L}} | {{Strategy}} |
```

</template_example>

<workflow_integration>
工作流程参考模板如下：

```xml
<process>
## Step 3: Generate Plan

1. Read `templates/plan-template.md`
2. Copy the template structure
3. Fill each placeholder based on gathered requirements
4. Review for completeness
</process>
```


工作流程告诉 Claude 何时使用该模板。该模板提供了要生成的结构。
</workflow_integration>

<best_practices>
**做：**
- 让模板专注于结构，而不是内容
- 一致地使用清晰的占位符语法
- 包括简短的内联指南，其中部分可能不明确
- 使模板完整但最少

**不要：**
- 放置过多可能被逐字复制的示例内容
- 为真正需要创意生成的输出创建模板
- 过度约束，所需部分过多
- 当需求发生变化时忘记更新模板
</best_practices>