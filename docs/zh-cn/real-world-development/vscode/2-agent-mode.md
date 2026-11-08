---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行测试。开发人员需要找到所有相关文件、进行更改、运行测试、调试并确认没有遗漏。Copilot 代理模式可以在集成开发环境 (IDE) 中更自主地处理这些工作：先探索项目结构，再协调更改、运行任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加功能。

> [!NOTE]
> 代理模式和云智能体名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，支持快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，你将了解如何：

- 让 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 使用 Copilot 代理模式跨用户界面 (UI) 和数据层实现功能。
- 在合并前审查 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表扩大，你希望允许用户按类别和发行方筛选。上一练习已添加 publishers helper，现在将使用 Copilot 代理模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

修改网站前，先了解当前功能。

网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示其详情。它是一个 Astro 应用，构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 任务，用于启动单个 Astro 开发服务器。

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 更新 `main` 并创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否在浏览器中打开 `http://localhost:4321`，选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是托管 Web 服务器的 Codespace 私有隧道。

### 探索网站

网站运行后，查看其主要功能：

- **Home Page**：显示桌游列表，包括标题、图片和说明。
- **Game Details Page**：选择游戏后，会打开详情页并显示标题、说明、发行方和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动干净的对话。模式和模型选择因练习而异；每个相关步骤都会说明。

模板创建的项目已包含待办议题。让 GitHub Copilot 显示这些项目并帮助确定优先级。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。Agent 可自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)
2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某一模型。构建本研讨会时使用了 Claude Sonnet 4.5，因此此处列出该模型，以便尽量保持建议的一致性。大型语言模型 (LLM) 具有概率性，因此 Copilot 的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的模型上下文协议 (MCP) 命令，但仍应能完成任务。

3. 发送以下提示，询问 Copilot 有哪些待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 会根据它认为对用户最有用的程度，为待办事项排序。

## 审查指令文件

让代理生成代码前，先查看用于提供工作上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等特定文件类型指令的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指令、拆分复杂指导，并提供示例和模板。

## 实现筛选功能

完成筛选需要至少三类应用更改：在 `src/lib/` 中添加或完善筛选逻辑、添加或更新筛选测试，以及更新游戏列表页并加入筛选 UI。合并前还必须运行并通过测试。Copilot 代理模式可以帮助完成这些工作。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)
4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂度，这可能需要几分钟。工作期间，编辑器可能会显示 **Keep** 和 **Undo**；完成后，每项更改都会提供这两个选项，无需在智能体工作过程中立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 要求确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)
8. 处理中可能会暂停，也可能有测试失败。Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完成的聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)
9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

你已使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。在本练习中，你了解了如何：

- 使用 Copilot 代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、找到相关文件并协调更改。
- 在合并前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 扩展练习——实现分页

游戏列表增多后需要分页。使用本练习中学到的技能，提示 Copilot 为网站添加分页。考虑采用何种分页方式、是否让用户选择每页大小，以及如何在提示中提供必要指导。可能需要与 Copilot 多次迭代、补充上下文，这是正常工作流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
