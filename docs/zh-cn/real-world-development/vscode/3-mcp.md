---
title: "练习 3 - 使用 Playwright 的模型上下文协议 (MCP) 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用代理模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。现在连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

在本练习中，你将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够访问编辑器以外的工具时，能力会大幅增强。模型上下文协议 (MCP) 为 Copilot 提供了与外部工具和服务通信的标准方式。

![示意图显示代理模式如何与上下文、LLM 和工具交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 智能体能够与外部工具和服务通信，从而实时访问最新信息（使用资源）并代表你执行操作（使用工具）。

人工智能 (AI) 智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当智能体与外部服务之间的桥梁，负责管理通信；外部服务可以是现有 API，也可以是 NPM 包等本地工具。每个 MCP 服务器都提供一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，例如浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 使用 MCP 服务器时，应像使用项目中的其他依赖项一样谨慎。使用前请仔细审查源代码、验证发布者并评估安全影响。只使用信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区可使用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 确认其中已配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

该 `github` 配置由项目模板提供，因此上一练习中 Copilot 能读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用其工具时需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁添加 `playwright` 配置，使文件如下所示：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 参数让 Playwright 在后台运行浏览器；Codespace 没有可显示浏览器的桌面，因此需要此参数。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 执行端到端测试，通常已安装浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器：Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 的 `playwright` 配置上方选择 **Start**，立即启动服务器并确认配置有效。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤；如果跳过，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器并提示信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供真实浏览器。智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结，而无需你亲自操作应用。这是在对话中快速确认功能行为的方法。

Playwright MCP 服务器基于页面的[无障碍树][playwright-mcp-server]而非截图工作。智能体会处理带标签的结构化元素（按钮、链接和列表项），与辅助技术类似，因此功能检查也可以顺便验证部分无障碍行为。

服务器已连接且应用正在运行时，让 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 启动浏览器，逐步执行检查并报告结果。根据议题中的验收标准审查报告；如果发现异常，继续提问或让 Copilot 修复代码，再创建 PR。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以发布 `filtering-vscode` 分支并创建拉取请求，供团队审查。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板显示 Copilot 更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让 Copilot 创建 PR，它可以找到相关议题并将 PR 与议题关联。

1. 打开 Copilot Chat 面板并选择 **New Chat**，启动新会话。
2. 在智能体下拉列表中选择 **Agent**，让 Copilot 能够使用 GitHub 工具。
3. 使用以下提示请求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示你登录 GitHub；按提示完成身份验证。
5. 确认 Copilot 搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但**暂时不要合并**。

## 总结和后续步骤

在本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何使用新工具扩展 Copilot。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试了筛选功能。
- 发布了分支并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
