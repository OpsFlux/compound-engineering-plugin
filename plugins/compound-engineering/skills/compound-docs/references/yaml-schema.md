# YAML Frontmatter 架构

**完整的模式规范请参见`.claude/skills/codify-docs/schema.yaml`。**

## 必填字段

- **模块**（字符串）：模块名称（例如，“EmailProcessing”）或“CORA”用于系统范围的问题
- **日期**（字符串）：ISO 8601 日期 (YYYY-MM-DD)
- **problem_type**（枚举）：[build_error、test_failure、runtime_error、performance_issue、database_issue、security_issue、ui_bug、integration_issue、logic_error、developer_experience、workflow_issue、best_practice、documentation_gap]之一
- **组件**（枚举）：[rails_model、rails_controller、rails_view、service_object、background_job、数据库、frontend_stimulus、hotwire_turbo、email_processing、brief_system、assistant、authentication、 payment、development_workflow、testing_framework、documentation、tooling]之一
- **症状**（数组）：1-5 个特定的可观察症状
- **root_cause**（枚举）：[missing_association、missing_include、missing_index、wrong_api、scope_issue、thread_violation、async_timing、memory_leak、config_error、logic_error、test_isolation、missing_validation、missing_permission、missing_workflow_step、inspection_documentation、missing_tooling、incomplete_setup]之一
- **分辨率类型**（枚举）：[code_fix、migration、config_change、test_fix、dependency_update、environment_setup、workflow_improvement、documentation_update、tooling_addition、seed_data_update]之一
- **严重性**（枚举）：[严重、高、中、低]之一

## 可选字段

- **rails_version** （字符串）：X.Y.Z 格式的 Rails 版本
- **标签**（数组）：可搜索关键字（小写，连字符分隔）

## 验证规则

1. 所有必填字段必须存在
2. 枚举字段必须与允许的值完全匹配（区分大小写）
3. 症状必须是包含 1-5 项的 YAML 数组
4.日期必须符合YYYY-MM-DD格式
5.rails_version（如果提供）必须匹配 X.Y.Z 格式
6.标签应该小写，用连字符分隔

## 例子

```yaml
---
module: Email Processing
date: 2025-11-12
problem_type: performance_issue
component: rails_model
symptoms:
  - "N+1 query when loading email threads"
  - "Brief generation taking >5 seconds"
root_cause: missing_include
rails_version: 7.1.2
resolution_type: code_fix
severity: high
tags: [n-plus-one, eager-loading, performance]
---
```


## 类别映射

根据 `problem_type`，文件归档于：

- **构建错误** → `docs/solutions/build-errors/`
- **测试失败** → `docs/solutions/test-failures/`
- **运行时错误** → `docs/solutions/runtime-errors/`
- **性能问题** → `docs/solutions/performance-issues/`
- **数据库问题** → `docs/solutions/database-issues/`
- **安全问题** → `docs/solutions/security-issues/`
- **ui_bug** → `docs/solutions/ui-bugs/`
- **集成问题** → `docs/solutions/integration-issues/`
- **逻辑错误** → `docs/solutions/logic-errors/`
- **开发者经验** → `docs/solutions/developer-experience/`
- **工作流程问题** → `docs/solutions/workflow-issues/`
- **最佳实践** → `docs/solutions/best-practices/`
- **documentation_gap** → `docs/solutions/documentation-gaps/`