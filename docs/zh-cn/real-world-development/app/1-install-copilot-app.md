---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的仓库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此仓库、分支和持续集成 (CI) 管道都可以直接使用。它适用于同时指挥多个智能体的工作流，每个智能体在自己的隔离工作区中运行；你无需亲自完成所有工作，还可以自动执行重复任务。Node.js 已安装、项目副本已准备就绪，现在安装应用并连接仓库。

本课将介绍如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 仓库将项目添加到应用。
- 熟悉工作区和模板预先创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正使用人工智能 (AI) 智能体处理不断增加的待办事项。Copilot app 将这些工作集中到一个地方：领取议题、运行智能体、审查更改并合并拉取请求。本课会带你完成安装和连接，并熟悉如何就项目发起对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略处于启用状态。

## 安装并配置 GitHub Copilot app

GitHub Copilot app 支持 Windows、macOS 和 Linux。现在安装应用、完成身份验证并添加 Tailspin Toys 仓库。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按页面说明安装。
3. 打开应用。
4. 选择 **Sign in to GitHub** 并按提示完成身份验证。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程提供仓库选择，请选择从模板创建的 Tailspin Toys 仓库。也可以跳过选择，在引导流程结束后再添加项目。
6. 按提示选择主题，然后完成引导流程。
7. 在侧边栏的 **Sessions** 下查找仓库。如果已经列出，无需重复添加。
8. 如果仓库尚未连接，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记录的所有者和仓库名称。
9. 确认仓库出现在侧边栏中。此处使用的布局会将 `tailspin-toys` 等仓库文件夹归在 **Sessions** 下，每个仓库下列出单独的会话。

## 熟悉工作区

项目连接后，先熟悉侧边栏中的几个主要区域：

- **New**：启动项目会话，或选择 **Chat** 快速提问。
- **Pull requests**：通过应用的 GitHub 原生集成浏览、筛选、检查 CI 状态并审查 PR。
- **Issues**：浏览和筛选议题，或从议题启动会话。
- **Automations**：按计划或按需运行已保存的智能体任务，适用于待办列表、定期项目维护和其他重复工作。本研讨会总结会将其作为后续探索内容，而非额外练习。
- **Customize**：添加模型上下文协议 (MCP) 服务器、插件、技能和其他组件。本研讨会会用它配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需单独创建分支或工作区。首次开始聊天后，此区域才会显示。如果未显示，选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按仓库整理文件夹和智能体会话。选择 `tailspin-toys` 等仓库名称可打开仓库视图；选择其下的会话可返回对话。每个会话都在独立工作区中运行，因此可以并行处理多个任务，而不会发生更改冲突。添加仓库后，你会启动第一个会话。

随着研讨会继续，你将逐步探索工作区。

> [!TIP]
> 有疑问就问 Copilot！如果不确定如何操作或某件事是否可行，可以向 Copilot 提问，让它提供指导。

### 查找预置的待办事项

Tailspin Toys 的模板已创建待办议题。按标题查找以下议题，不要假设它们的议题编号：

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 仓库文件夹，而不是其下的某个会话。然后在仓库顶部导航中选择 **Issues**。
2. 查找以下议题：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题并阅读详情。每个议题也可以作为智能体会话的起点。完成第一次小更改后，将从筛选功能议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 列表会自动筛选，只显示已添加到 Copilot app 的仓库中的项目。要查看其他仓库的工作项，请将它们添加到应用。

## 尝试快速聊天

快速聊天适合用来了解应用本身。它可以让你提问或集思广益，而无需创建分支或工作树，适合快速的一次性问题。

1. 在侧边栏中，选择 **Chats** 旁的 **+** 打开新聊天。如果 **Chats** 尚未显示，选择 **Sessions** 旁的 **+** 开始第一次聊天；之后 **Chats** 就会出现。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 Git 工作树中运行，因此可以并行运行多个智能体而不发生更改冲突。你可以继续对话，也可以随时开始新聊天。

## 总结和后续步骤

你已安装 GitHub Copilot app、连接项目并探索工作区。在本课中，你了解了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 仓库添加项目。
- 熟悉工作区，并在 **Issues** 中查找预置的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
