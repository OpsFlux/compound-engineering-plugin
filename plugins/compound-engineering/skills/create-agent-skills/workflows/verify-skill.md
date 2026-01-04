# 工作流程：验证技能内容的准确性

<required_reading>
**立即阅读这些参考文件：**
1.references/skill-structure.md
</required_reading>

<purpose>
审计检查结构。 **验证检查真实性。**

技能包含有关外部事物的声明：API、CLI 工具、框架、服务。这些随着时间的推移而改变。此工作流程检查技能内容是否仍然准确。
</purpose>

<process>
## 第 1 步：选择技能

```bash
ls ~/.claude/skills/
```


出示编号列表，询问：“我应该验证哪项技能的准确性？”

## 第 2 步：阅读并分类

阅读整个技能（SKILL.md + 工作流程/ + 参考资料/）：
```bash
cat ~/.claude/skills/{skill-name}/SKILL.md
cat ~/.claude/skills/{skill-name}/workflows/*.md 2>/dev/null
cat ~/.claude/skills/{skill-name}/references/*.md 2>/dev/null
```


按主要依赖类型分类：

|类型 |示例 |验证方法|
|------|----------|---------------------|
| **API/服务** |管理条纹，管理高级 | Context7 + 网络搜索 |
| **CLI 工具** |构建-macos-apps（xcodebuild，swift）|运行命令|
| **框架** |构建 iPhone 应用程序（SwiftUI、UIKit）|文档的 Context7 |
| **整合** |设置条带付款| WebFetch + Context7 |
| **纯工艺** |创建代理技能 |没有外部部门 |

报告：“该技能主要是基于[类型]的。我将使用[方法]进行验证。”

## 步骤 3：提取可验证的声明

扫描技能内容并提取：

**提到的 CLI 工具：**
- 工具名称（xcodebuild、swift、npm 等）
- 记录特定标志/选项
- 预期输出模式

**API端点：**
- 服务名称（Stripe、Meta 等）
- 记录特定端点
- 认证方式
- SDK版本

**框架模式：**
- 框架名称（SwiftUI、React 等）
- 记录特定 API/模式
- 版本特定的功能

**文件路径/结构：**
- 预期的项目结构
- 配置文件位置

目前：“发现 X 个可验证的声明需要检查。”

## 步骤 4：按类型验证

### 对于 CLI 工具
```bash
# Check tool exists
which {tool-name}

# Check version
{tool-name} --version

# Verify documented flags work
{tool-name} --help | grep "{documented-flag}"
```


### API/服务技能
使用 Context7 获取当前文档：
```
mcp__context7__resolve-library-id: {service-name}
mcp__context7__get-library-docs: {library-id}, topic: {relevant-topic}
```


将技能记录的模式与当前文档进行比较：
- 端点仍然有效吗？
- 身份验证是否已更改？
- 是否使用了已弃用的方法？

### 对于框架技能
使用上下文7：
```
mcp__context7__resolve-library-id: {framework-name}
mcp__context7__get-library-docs: {library-id}, topic: {specific-api}
```


检查：
- 记录的 API 仍然是最新的吗？
- 模式有变化吗？
- 有更新的推荐方法吗？

### 整合技能
网络搜索最近的更改：
```
"[service name] API changes 2025"
"[service name] breaking changes"
"[service name] deprecated endpoints"
```


然后是当前 SDK 模式的 Context7。

### 对于带有状态页面的服务
WebFetch 官方文档/变更日志（如果有）。

## 步骤 5：生成新鲜度报告

目前的发现：

```
## Verification Report: {skill-name}

### ✅ Verified Current
- [Claim]: [Evidence it's still accurate]

### ⚠️ May Be Outdated
- [Claim]: [What changed / newer info found]
  → Current: [what docs now say]

### ❌ Broken / Invalid
- [Claim]: [Why it's wrong]
  → Fix: [What it should be]

### ℹ️ Could Not Verify
- [Claim]: [Why verification wasn't possible]

---
**Overall Status:** [Fresh / Needs Updates / Significantly Stale]
**Last Verified:** [Today's date]
```


## 第 6 步：优惠更新

如果发现问题：

“发现 [N] 个需要更新的项目。您希望我：”

1. **全部更新** - 应用所有更正
2. **查看每个** - 在应用之前显示每个更改
3. **仅报告** - 没有变化

如果更新：
- 根据已验证的当前信息进行更改
- 添加验证日期注释（如果适用）
- 报告更新内容

## 步骤 7：建议验证时间表

根据技能类型，推荐：

|技能类型 |推荐频率 |
|------------|------------------------|
| API/服务 |每 1-2 个月 |
|框架|每 3-6 个月 |
| CLI 工具 |每 6 个月 |
|纯工艺|每年 |

“这项技能应该在大约[时间范围]内重新验证。”
</process>

<verification_shortcuts>
## 快速验证命令

**检查CLI工具是否存在并获取版本：**
```bash
which {tool} && {tool} --version
```


**适用于任何库的 Context7 模式：**
```
1. resolve-library-id: "{library-name}"
2. get-library-docs: "{id}", topic: "{specific-feature}"
```


**网络搜索模式：**
- 重大变更：“{service} 2025 年重大变更”
- 弃用：“{service} 已弃用 API”
- 当前最佳实践：“{framework} 2025 年最佳实践”
</verification_shortcuts>

<success_criteria>
验证完成时：
- [ ] 按依赖类型分类的技能
- [ ] 提取可验证的声明
- [ ] 每项索赔均采用适当的方法进行检查
- [ ] 生成新鲜度报告
- [ ] 应用更新（如果需要）
- [ ] 用户知道何时重新验证
</success_criteria>