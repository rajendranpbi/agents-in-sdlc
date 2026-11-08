---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你使用 GitHub Copilot 完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建相关游戏功能，以及实现高对比度和浅色模式。现在审查代码更改；必要时向 Copilot 提供反馈，改进其工作。

### 场景

引入生成式人工智能 (AI) 并不会改变软件设计和开发运维 (DevOps) 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。现在依次审查 Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，然后再为团队开启审查。

## GitHub Copilot 云智能体与安全

Copilot 云智能体会异步且无人监督地执行任务，因此设置了安全限制：

- Copilot 只有仓库读取权限，并且只能写入它用于提交代码的分支。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制][agent-firewall]，包括模型上下文协议 (MCP) 服务器。

## 审查生成的代码文档

先通过 GitHub.com 上的标准拉取请求 (PR) 界面，查看 GitHub Copilot 云智能体创建的第一个 PR，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为默认情况下 Copilot 对外部资源的访问受限，包括调用外部 MCP 服务器。你可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡，审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的 TSDoc 文档注释和其他文档。具体更改可能不同。留意 TSDoc 注释、TypeScript 约定和文件注释头，它们来自练习 2 中查看的自定义指令文件。
6. 审查完毕后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域。对于单个 Astro 应用，项目检查应全部通过；这可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以在 PR 评论或代码行内评论中像提及团队成员一样提及 Copilot。Copilot 会读取评论，并启动新会话处理反馈。由于结果具有非确定性，无法规定唯一的提示文本。可以要求 Copilot：

- 为每个代码文件添加简短的文件注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求它修改生成的文档。可以使用上述建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并执行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡，审查更改。满意后，可以将 PR 从草稿转换为可审查状态并合并到 main。

   ![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来审查 Copilot 为游戏详情页添加相关游戏功能而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 检查 PR 的检查区域，确认单个 Astro 应用的项目检查通过。此步骤可能需要几分钟。
9. **可选：** 在 Codespace 中检出此分支，手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名（例如 `copilot/fix-8`），然后运行：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已实现相关游戏功能。与之前一样，可以通过在 **Conversation** 选项卡中添加评论来要求 Copilot 迭代，例如调整显示的相关游戏数量，或补充文件注释头和 TSDoc 注释。请记住，此议题是在更新自定义指令之前分配的。

## 审查无障碍功能

最后审查无障碍访问自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注：
   - 用于切换模式的用户界面 (UI) 组件。
   - 使用本地存储保存用户偏好。
   - 高对比度和浅色模式相关的层叠样式表 (CSS) 或其他样式更改。
   - 无障碍富互联网应用 (ARIA) 标签、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 确认更改符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 检查 PR 的检查区域，确认单个 Astro 应用的项目检查通过。此步骤可能需要几分钟。
10. **可选：** 在 Codespace 中检出该分支以手动测试功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名，然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    启动应用，在浏览器中测试高对比度和浅色模式切换，并确认页面重新加载后仍保留设置。

无障碍访问自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现问题或改进点，可像处理其他 PR 一样在评论中提及 **@copilot** 请求更新。

## 可选练习——继续委派任务

云智能体适合处理真实待办事项，让你能同时处理其他工作。可以在自己的仓库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境的练习。在这些课程中，你：

- **检查了仓库提供的自定义指令文件**，以便之后观察它们对云智能体结果的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成云智能体工作环境的练习。想继续探索时，可以了解其他工作环境：

- 🖥️ **[VS Code 工作环境](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)**：在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的仓库中尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
