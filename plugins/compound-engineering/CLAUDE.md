# 复合工程插件开发

## 版本控制要求

**重要**：对此插件的每次更改都必须包括对所有三个文件的更新：

1. **`.claude-plugin/plugin.json`** - 使用 semver 的凹凸版本
2. **`CHANGELOG.md`** - 使用保留变更日志格式记录变更
3. **`README.md`** - 验证/更新组件数量和表格

### 版本冲突规则

- **主要** (1.0.0 → 2.0.0)：重大变更、重大重组
- **次要** (1.0.0 → 1.1.0)：新代理、命令或技能
- **补丁** (1.0.0 → 1.0.1)：错误修复、文档更新、细微改进

### 预提交清单

在进行任何更改之前：

- [ ] `.claude-plugin/plugin.json` 中的版本已更改
- [ ] CHANGELOG.md 更新了更改
- [ ] README.md 组件计数已验证
- [ ] README.md 表准确（代理、命令、技能）
- [ ]plugin.json 描述与当前计数匹配

### 目录结构

```
agents/
├── review/     # Code review agents
├── research/   # Research and analysis agents
├── design/     # Design and UI agents
├── workflow/   # Workflow automation agents
└── docs/       # Documentation agents

commands/
├── workflows/  # Core workflow commands (workflows:plan, workflows:review, etc.)
└── *.md        # Utility commands

skills/
└── *.md        # All skills at root level
```


## 命令命名约定

**工作流程命令**使用 `workflows:` 前缀以避免与内置命令发生冲突：
- `/workflows:plan` - 制定实施计划
- `/workflows:review` - 运行全面的代码审查
- `/workflows:work` - 系统地执行工作项目
- `/workflows:compound` - 记录已解决的问题

**为什么`workflows:`?** Claude Code 具有内置的 `/plan` 和 `/review` 命令。在 frontmatter 中使用 `name: workflows:plan` 创建一个独特的 `/workflows:plan` 命令，不会发生冲突。

## 技能合规检查表

添加或修改技能时，请验证是否符合技能创建者规范：

### YAML Frontmatter（必需）

- [ ] `name:` 显示并匹配目录名称（小写字母加连字符）
- [ ] `description:` 呈现并使用**第三人称**（“该技能应在...时使用”而不是“在...时使用该技能”）

### 参考链接（如果引用/存在则为必填）

- [ ] `references/` 中的所有文件均链接为 `[filename.md](./references/filename.md)`
- [ ] `assets/` 中的所有文件均链接为 `[filename](./assets/filename)`
- [ ] `scripts/` 中的所有文件均链接为 `[filename](./scripts/filename)`
- [ ] 没有裸露的反引号引用，例如 `` `references/file.md` `` - 使用正确的降价链接

### 写作风格

- [ ] 使用命令式/不定式形式（动词在前的指令）
- [ ] 避免第二人称（“你应该”） - 使用客观语言（“为了完成 X，做 Y”）

### 快速验证命令

```bash
# Check for unlinked references in a skill
grep -E '`(references|assets|scripts)/[^`]+`' skills/*/SKILL.md
# Should return nothing if all refs are properly linked

# Check description format
grep -E '^description:' skills/*/SKILL.md | grep -v 'This skill'
# Should return nothing if all use third person
```


## 文档

有关详细的版本控制工作流程，请参阅`docs/solutions/plugin-versioning-requirements.md`。