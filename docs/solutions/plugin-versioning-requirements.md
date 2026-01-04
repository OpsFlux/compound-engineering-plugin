---
title: 插件版本控制与文档要求
category: workflow
tags: [versioning, changelog, readme, plugin, documentation]
created: 2025-11-24
severity: process
component: plugin-development
---
# 插件版本控制和文档要求

## 问题

对复合工程插件进行更改时，文档可能与实际组件（代理、命令、技能）不同步。这会导致人们对每个版本中包含的内容感到困惑，并且很难跟踪随时间的变化。

## 解决方案

**对插件的每项更改都必须包括：**

1. **`plugin.json`** 中的版本提升
   - 遵循语义版本控制（semver）
   - 主要：重大变更或重大重组
   - 次要：添加了新的代理、命令或技能
   - 补丁：错误修复、文档更新、小改进

2. **CHANGELOG.md 更新**
   - 在`## [Unreleased]`或新版本部分下添加条目
   - 使用保留变更日志格式
   - 类别：添加、更改、弃用、删除、修复、安全

3. **README.md验证**
   - 验证组件计数与实际文件匹配
   - 验证代理/命令/技能表是否准确
   - 如果功能发生变化则更新描述

## 插件更改清单

```markdown
Before committing changes to compound-engineering plugin:

- [ ] Version bumped in `.claude-plugin/plugin.json`
- [ ] CHANGELOG.md updated with changes
- [ ] README.md component counts verified
- [ ] README.md tables updated (if adding/removing/renaming)
- [ ] plugin.json description updated (if component counts changed)
```


## 文件位置

- 版本：`.claude-plugin/plugin.json` → `"version": "X.Y.Z"`
- 变更日志：`CHANGELOG.md`
- 自述文件：`README.md`

## 工作流程示例

添加新代理时：

1. 在`agents/[category]/`中创建代理文件
2. `plugin.json`中的凹凸版本（新代理的次要版本）
3. 添加到 `### Added` 下的变更日志
4. 将行添加到 README 代理表
5.更新README组件数量
6. 使用新计数更新 plugin.json 描述

## 预防

本文档起到提醒作用。当克劳德代码在这个插件上工作时，它应该：

1. 在提交更改之前检查此文档
2. 按照上面的清单进行操作
3. 切勿提交部分更新（所有三个文件必须一起更新）

## 相关文件

- `/Users/kieranklaassen/every-marketplace/plugins/compound-engineering/.claude-plugin/plugin.json`
- `/Users/kieranklaassen/every-marketplace/plugins/compound-engineering/CHANGELOG.md`
- `/Users/kieranklaassen/every-marketplace/plugins/compound-engineering/README.md`
