# 工作流程：向技能添加模板

<required_reading>
**立即阅读这些参考文件：**
1.references/using-templates.md
</required_reading>

<process>
## 第 1 步：确定技能

询问（如果尚未提供）：
- 哪些技能需要模板？
- 该模板构造什么输出？

## 步骤 2：分析模板需求

确认这是一个好的候选模板：
- [ ] 输出在不同用途中具有一致的结构
- [ ] 结构比创意生成更重要
- [ ] 填充占位符比生成空白页更可靠

如果不合适，请提出替代方案（工作流程指南、参考示例）。

## 第三步：创建模板目录

```bash
mkdir -p ~/.claude/skills/{skill-name}/templates
```


## 步骤4：设计模板结构

收集要求：
- 输出需要哪些部分？
- 哪些信息因用途而异？ （→ 占位符）
- 什么保持不变？ （→ 静态结构）

## 步骤5：编写模板文件

创建`templates/{template-name}.md`：
- 清晰的部分标记
- `{{PLACEHOLDER}}`可变内容语法
- 简短的内联指导（如有帮助）
- 最少的示例内容

## 步骤 6：更新工作流程以使用模板

找到产生此输出的工作流程。添加：
```xml
<process>
...
N. Read `templates/{template-name}.md`
N+1. Copy template structure
N+2. Fill each placeholder based on gathered context
...
</process>
```


## 步骤 7：测试

调用技能工作流程并验证：
- 在正确的步骤读取模板
- 所有占位符都得到适当填充
- 输出结构与模板匹配
- 没有未填写的占位符
</process>

<success_criteria>
模板在以下情况下完成：
- [ ] templates/ 目录存在
- [ ] 模板文件结构清晰，带有占位符
- [ ] 至少一个工作流程引用该模板
- [ ] 工作流程说明解释何时/如何使用模板
- [ ] 通过真实调用进行测试
</success_criteria>