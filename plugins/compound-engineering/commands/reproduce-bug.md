---
name: reproduce-bug
description: 使用日志和控制台检查重现和调查错误

argument-hint: "[GitHub issue number]"
---
查看 github 问题 #$ARGUMENTS 并阅读问题描述和评论。

然后，并行运行以下代理以重现该错误：

1.任务rails-console-explorer(issue_description)
2. 任务 appsignal-log-investigator (issue_description)

然后考虑一下查看代码库时可能出错的地方。寻找我们可以寻找的登录输出。

然后，再次并行运行以下代理以查找任何可以帮助我们重现错误的日志。

1.任务rails-console-explorer(issue_description)
2. 任务 appsignal-log-investigator (issue_description)

继续运行这些代理，直到您清楚地了解发生了什么。

**参考资料集：**

- [ ] 使用特定文件路径记录所有研究结果（例如，`app/services/example_service.rb:42`）

然后，向问题添加评论，其中包含调查结果以及如何重现错误。