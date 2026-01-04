# 复合工程插件

一个 Claude Code 插件，使每个工程单元的工作都比上一个单元更容易。

## 安装

```bash
/plugin marketplace add https://github.com/EveryInc/compound-engineering-plugin
/plugin install compound-engineering
```


## 工作流程

```
Plan → Work → Review → Compound → Repeat
```


|命令 |目的|
|---------|---------|
| `/workflows:plan` |将功能构想转化为详细的实施计划 |
| `/workflows:work` |使用工作树和任务跟踪执行计划 |
| `/workflows:review` |合并前的多代理代码审查 |
| `/workflows:compound` |记录学习内容，让未来的工作更轻松 |

每个周期都会复合：计划为未来计划提供信息，审查发现更多问题，模式被记录下来。

## 哲学

**工程工作的每个单元都应该使后续单元变得更容易，而不是更难。**

传统开发会积累技术债务。每个功能都会增加复杂性。随着时间的推移，代码库变得越来越难以使用。

复合工程扭转了这一点。 80%在计划和审查，20%在执行：
- 编写代码之前彻底计划
- 回顾以发现问题并汲取经验教训
- 整理知识，使其可重复使用
- 保持高品质，以便将来的更改变得容易

## 了解更多

- [完整组件参考](plugins/compound-engineering/README.md) - 所有代理、命令、技能
- [复合工程：Every 如何使用代理进行编码](https://every.to/chain-of-thought/compound-engineering-how-every-codes-with-agents)
- [复合工程背后的故事](https://every.to/source-code/my-ai-had-already-fixed-the-code-before-i-saw-it)