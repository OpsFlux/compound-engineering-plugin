---
module: [Module name or "CORA" for system-wide]
date: [YYYY-MM-DD]
problem_type: [build_error|test_failure|runtime_error|performance_issue|database_issue|security_issue|ui_bug|integration_issue|logic_error]
component: [rails_model|rails_controller|rails_view|service_object|background_job|database|frontend_stimulus|hotwire_turbo|email_processing|brief_system|assistant|authentication|payments]
symptoms:
  - [Observable symptom 1 - specific error message or behavior]
  - [Observable symptom 2 - what user actually saw/experienced]
root_cause: [missing_association|missing_include|missing_index|wrong_api|scope_issue|thread_violation|async_timing|memory_leak|config_error|logic_error|test_isolation|missing_validation|missing_permission]
rails_version: [7.1.2 - optional]
resolution_type: [code_fix|migration|config_change|test_fix|dependency_update|environment_setup]
severity: [critical|high|medium|low]
tags: [keyword1, keyword2, keyword3]
---
# 故障排除：【清除问题标题】

## 问题
[1-2 句话清楚地描述问题和用户经历的情况]

## 环境
- 模块：[名称或“CORA 系统”]
- Rails 版本：[例如 7.1.2]
- 受影响的组件：[例如，“电子邮件处理模型”、“简要系统服务”、“身份验证控制器”]
- 日期：[问题解决时的 YYYY-MM-DD]

## 症状
- [可观察到的症状 1 - 用户所看到/经历的]
- [观察到的症状 2 - 错误消息、视觉问题、意外行为]
- [根据需要继续 - 具体]

## 什么不起作用

**尝试的解决方案 1：** [尝试过的内容的描述]
- **为什么失败：** [这没有解决问题的技术原因]

**尝试的解决方案2：** [第二次尝试的说明]
- **为什么失败：** [技术原因]

[继续进行所有无效的重要尝试]

[如果没有先尝试其他方法，请写：]
**直接解决方案：** 问题在第一次尝试时就被识别并修复。

## 解决方案

[实际有效的修复 - 提供具体细节]

**代码更改**（如果适用）：
```ruby
# Before (broken):
[Show the problematic code]

# After (fixed):
[Show the corrected code with explanation]
```


**数据库迁移**（如果适用）：
```ruby
# Migration change:
[Show what was changed in the migration]
```


**运行命令**（如果适用）：
```bash
# Steps taken to fix:
[Commands or actions]
```


## 为什么这有效

[技术说明：]
1. 问题的根本原因是什么？
2. 为什么该解决方案能够解决这个根本原因？
3. 根本问题是什么（API 误用、配置错误、Rails 版本问题等）？

[足够详细，以便未来的开发人员了解“为什么”，而不仅仅是“什么”]

## 预防

【今后CORA开发中如何避免这个问题：】
- [具体的编码实践、检查或遵循的模式]
- [注意事项]
- [如何尽早赶上]

## 相关问题

[如果 docs/solutions/ 中存在任何类似问题，请链接到它们：]
- See also: [another-related-issue.md](../category/another-related-issue.md)
- 类似于：[相关问题.md](../category/related-problem.md)

[如果没有相关问题，请写：]
尚未记录相关问题。