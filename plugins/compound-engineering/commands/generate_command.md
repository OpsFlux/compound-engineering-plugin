---
name: generate_command
description: 遵循约定和最佳实践创建新的自定义斜杠命令

argument-hint: "[command purpose and requirements]"
---
# 创建自定义 Claude 代码命令

在 `.claude/commands/` 中为请求的任务创建一个新的斜杠命令。

## 目标

# $参数

## 可利用的关键功能

**文件操作：**
- 读取、编辑、写入 - 精确修改文件
- Glob、Grep - 搜索代码库
- MultiEdit - 原子多部分更改

**发展：**
- Bash - 运行命令（git、测试、linter）
- 任务 - 启动专门的代理来执行复杂的任务
- TodoWrite - 使用待办事项列表跟踪进度

**网络和 API：**
- WebFetch、WebSearch - 研究文档
- GitHub (gh cli) - PR、问题、评论
- 剧作家 - 浏览器自动化、屏幕截图

**集成：**
- AppSignal - 日志和监控
- Context7 - 框架文档
- Stripe、Todoist、Featurebase（如果相关）

## 最佳实践

1. **具体、清晰** - 详细的说明会产生更好的结果
2. **分解复杂的任务** - 使用分步计划
3. **使用示例** - 引用现有的代码模式
4. **包括成功标准** - 测试通过、掉毛干净等。
5. **先思考** - 对于复杂问题使用“认真思考”或“计划”关键字
6. **迭代** - 逐步指导流程

## 必需：YAML Frontmatter

**每个命令必须以 YAML frontmatter 开头：**

```yaml
---
name: command-name
description: Brief description of what this command does (max 100 chars)
argument-hint: "[what arguments the command accepts]"
---
```


**字段：**
- `name`：小写命令标识符（内部使用）
- `description`：命令目的的清晰、简洁的总结
- `argument-hint`：向用户显示需要哪些参数（例如，`[file path]`、`[PR number]`、`[optional: format]`）

## 构建你的命令

```markdown
# [Command Name]

[Brief description of what this command does]

## Steps

1. [First step with specific details]
   - Include file paths, patterns, or constraints
   - Reference existing code if applicable

2. [Second step]
   - Use parallel tool calls when possible
   - Check/verify results

3. [Final steps]
   - Run tests
   - Lint code
   - Commit changes (if appropriate)

## Success Criteria

- [ ] Tests pass
- [ ] Code follows style guide
- [ ] Documentation updated (if needed)
```


## 有效命令的技巧

- **使用 $ARGUMENTS** 占位符进行动态输入
- **参考 CLAUDE.md** 模式和约定
- **包括验证步骤** - 测试、检查、目视检查
- **明确约束** - 不要修改 X，使用模式 Y
- **使用 XML 标签**进行结构化提示：`<task>`、`<requirements>`、`<constraints>`

## 示例模式

```markdown
Implement #$ARGUMENTS following these steps:

1. Research existing patterns
   - Search for similar code using Grep
   - Read relevant files to understand approach

2. Plan the implementation
   - Think through edge cases and requirements
   - Consider test cases needed

3. Implement
   - Follow existing code patterns (reference specific files)
   - Write tests first if doing TDD
   - Ensure code follows CLAUDE.md conventions

4. Verify
   - Run tests: `bin/rails test`
   - Run linter: `bundle exec standardrb`
   - Check changes with git diff

5. Commit (optional)
   - Stage changes
   - Write clear commit message
```


## 创建命令文件

1. **在`.claude/commands/[name].md`处创建文件**（支持`workflows/`等子目录）
2. **从 YAML frontmatter 开始**（参见上面的部分）
3. **使用上面的模板构建命令**
4. **通过使用适当的参数来测试命令**

## 命令文件模板

```markdown
---
name: command-name
description: What this command does
argument-hint: "[expected arguments]"
---

# Command Title

Brief introduction of what the command does and when to use it.

## Workflow

### Step 1: [First Major Step]

Details about what to do.

### Step 2: [Second Major Step]

Details about what to do.

## Success Criteria

- [ ] Expected outcome 1
- [ ] Expected outcome 2
```

