---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如未解决的安全问题、需要更新的旧代码，或长期搁置的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。任务完成后，它会创建待审查的草稿拉取请求 (PR)，让开发人员可以委派繁琐任务，专注于更重要的工作。

本练习将探索如何：

- 自定义代码生成环境。
- 确保操作以安全的方式执行。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。承包商最初开发网站时没有为代码编写文档。团队希望先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上显示同类别的其他游戏，以便开展用户体验探索和验收测试。这项工作目前受阻，但有其他优先级更高的议题。

这类任务很容易被推迟，适合交给云智能体异步处理。开发人员可以继续处理其他工作，之后再审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你可以通过[分配 GitHub 议题][assign-issue]等方式交办工作。分配后，Copilot 会创建草稿 PR 跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情；当解决方案准备好供审查时，Copilot 会在 PR 中通知你。

## 明确指令和任务范围

GitHub Copilot 并非魔法，使用它并不能省去思考。即使看似简单的操作，也可能包含复杂情况。应谨慎界定云智能体任务范围，并像使用 AI 配对编程助手一样分阶段工作、学习、试验和调整。生成式 AI 不会改变软件开发的基本原则。

## 本仓库中的自定义指令

先以只读方式查看仓库中已有的指令文件，稍后可观察它们的影响。

1. 在 GitHub Web UI 中打开仓库，或在已启动的 Codespace 中打开 `.github/copilot-instructions.md`。查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
2. 打开 `.github/instructions/unit-tests.instructions.md`，查看将指令限定到 `**/*.test.ts` 文件的 `applyTo` frontmatter。
3. 将议题 `Code lacks documentation` 分配给云智能体时，留意它生成的 PR 是否遵循 TSDoc、TypeScript 约定和文件注释头。这些要求来自上述指令文件，之后审查 PR 时还会再次检查。

## 为云智能体设置开发环境

云智能体通过 [GitHub Actions][github-actions] 设置工作环境。可在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在 Copilot 开始工作前安装开发工具和依赖项。实验仓库已预先配置此工作流，为单个 Astro 应用安装 Node.js、项目依赖、端到端测试所需的浏览器，并迁移和填充 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从仓库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低范围。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆仓库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆仓库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

此工作流包含一个名为 `copilot-setup-steps` 的作业，会在 Copilot 开始处理 PR 前运行。`workflow_dispatch` 触发器允许你在仓库的 **Actions** 选项卡中手动运行工作流，以便在 Copilot 使用前检查它是否成功。

## 添加代码文档

代码文档对项目维护和新成员上手很重要，但常常过时或缺失。Copilot 可以在后台生成文档，适合处理这类技术债务；之后再审查其工作。

1. 在新浏览器标签页中打开 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
   
   ![Copilot 分配议题的详细信息](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡并打开新生成的 PR。标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新页面。
11. 几分钟后，Copilot 应会创建待办事项清单。

> [!NOTE]
> 待办事项清单可能需要几分钟才会显示，因为 Copilot 正在创建环境、运行前面介绍的工作流、分析项目并确定处理方案。

12. 审查清单及 Copilot 即将执行的任务。
13. 在 PR 时间线中向下滚动，确认 Copilot 已开始处理议题。
14. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

15. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、确定方案并编写代码。

这可能需要几分钟。云智能体的优势之一是异步工作，让你能同时处理其他任务。接下来利用这一点，再分配一项任务，然后转而编写应用功能。

## 为游戏详情页添加相关游戏

云智能体可以并行处理不同任务，让你专注于自己的工作。现在将设计团队的相关游戏需求也分配给它。

1. 返回 github.com 上的仓库，选择 **Issues** 选项卡。
2. 选择 **New issue**，再选择 **Blank issue**。
3. 在 **Title** 中输入 `Show related games on the game details page`。
4. 在 **Description** 中输入：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. 选择 **Create** 创建议题。
6. 在右侧选择 **Assign to Copilot**，打开分配对话框。
7. 选择 **Assign**。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需主动监控；完成后再审查即可。现在可以继续了解自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给 Copilot，让它异步处理技术债务、开发功能或协助迁移代码。

在本课中，你：

- 自定义了代码生成环境。
- 了解了如何安全地执行操作。
- 了解了明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，可以继续了解如何创建和使用自定义智能体。云智能体也支持[模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
