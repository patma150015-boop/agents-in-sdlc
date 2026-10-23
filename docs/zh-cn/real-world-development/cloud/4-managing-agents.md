---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体承担了三项改善用户体验和增加功能的任务。尽管云智能体会异步自主工作，仍需监控这些任务。

可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在此任务控制中心可以查看所有已创建开放拉取请求 (PR) 的智能体任务、检查执行情况，并引导正在进行的会话。

本练习将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应添加浅色模式。既然已经在更新网站样式并添加切换功能，顺便实现浅色模式也很合适。现在引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，例如：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口 (API)。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，包括：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议 (MCP) 服务器。
   - 执行完成任务所需的步骤。

> [!NOTE]
> 列出的具体步骤取决于 Copilot 当前工作状态和采用的方式。

4. 还可以查看右侧显示的拉取请求 (PR) 面板，了解 PR 和更改文件。

## 引导云智能体

已查看当前活动任务，现在要求 Copilot 在实现高对比度模式的同时添加浅色模式开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始执行任务。看到类似以下消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中显示云智能体任务及引导活动会话对话框的截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词并将其加入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务。它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

这可能需要几分钟，可以趁此机会回顾目前学到和探索的内容。

## 总结与后续步骤

本练习介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心。可通过此任务控制中心跟踪所有活动和已完成任务、审查正在进行的工作，并重定向进行中的任务以调整范围或补充指导。

本练习中，你：

- 使用任务控制中心和智能体页面监控了云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
