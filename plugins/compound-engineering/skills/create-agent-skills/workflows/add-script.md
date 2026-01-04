# 工作流程：将脚本添加到技能中

<required_reading>
**立即阅读这些参考文件：**
1.references/using-scripts.md
</required_reading>

<process>
## 第 1 步：确定技能

询问（如果尚未提供）：
- 哪些技能需要脚本？
- 脚本应该执行什么操作？

## 步骤 2：分析脚本需求

确认这是一个好的候选脚本：
- [ ] 相同的代码在多次调用中运行
- [ ] 重写时操作容易出错
- [ ] 一致性比灵活性更重要

如果不合适，请建议替代方案（工作流程中的内联代码、参考示例）。

## 第三步：创建脚本目录

```bash
mkdir -p ~/.claude/skills/{skill-name}/scripts
```


## 步骤 4：设计脚本

收集要求：
- 脚本需要什么输入？
- 它应该输出或完成什么？
- 可能会出现什么错误？
- 它应该是幂等的吗？

选择语言：
- **bash** - Shell 操作、文件操作、CLI 工具
- **python** - 数据处理、API调用、复杂逻辑
- **node/ts** - JavaScript 生态系统，异步操作

## 步骤5：编写脚本文件

创建`scripts/{script-name}.{ext}`：
- 顶部的目的评论
- 使用说明
- 输入验证
- 错误处理
- 清晰的输出/反馈

对于 bash 脚本：
```bash
#!/bin/bash
set -euo pipefail
```


## 步骤 6：制作可执行文件（如果是 bash）

```bash
chmod +x ~/.claude/skills/{skill-name}/scripts/{script-name}.sh
```


## 步骤 7：更新工作流程以使用脚本

找到需要此操作的工作流程。添加：
```xml
<process>
...
N. Run `scripts/{script-name}.sh [arguments]`
N+1. Verify operation succeeded
...
</process>
```


## 步骤 8：测试

调用技能工作流程并验证：
- 脚本在正确的步骤运行
- 输入正确传递
- 错误得到妥善处理
- 输出符合预期
</process>

<success_criteria>
脚本完成时：
- [ ] 脚本/目录存在
- [ ] 脚本文件具有正确的结构（注释、验证、错误处理）
- [ ] 脚本可执行（如果是 bash）
- [ ] 至少一个工作流程引用该脚本
- [ ] 没有硬编码的秘密或凭证
- [ ] 通过真实调用进行测试
</success_criteria>