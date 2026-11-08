---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体处理了三项改善用户体验和添加功能的任务。云智能体会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有带有未关闭拉取请求 (PR) 的智能体任务，检查已执行的操作，并引导正在进行的会话。

在本课中，你将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队决定同时添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，顺便实现浅色模式很合适。现在引导智能体在处理高对比度模式时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表，其中应包含更新代码文档、生成游戏修改 API，以及为网站添加高对比度模式等任务。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如检出代码、创建工作环境、设置模型上下文协议 (MCP) 服务器，以及完成任务的其他步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 查看右侧显示的 PR 面板，以便进一步监控 PR 和更改的文件。

## 引导云智能体

查看当前任务后，请求 Copilot 在实现高对比度模式时也添加浅色模式。

1. 选择与添加高对比度模式有关的会话。会话标题取决于 Copilot 使用的名称和当前工作状态。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话显示设置已完成并开始执行任务。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的云智能体任务截图，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 确认 Copilot 已接收提示并将其纳入工作流程。

## 让 Copilot 继续工作

Copilot 会在完成发送提示时正在执行的步骤后，将新要求纳入工作流程。等待期间，可以回顾目前学到的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以跟踪所有正在进行和已完成的任务、审查工作，也可以调整正在进行任务的范围或补充指导。

在本课中，你：

- 探索了任务控制中心和智能体页面，以监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍访问功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
