---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI、完成身份验证、熟悉环境并查找预置的筛选功能议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一款功能强大的代理式编码助手，可在终端运行，让你通过命令行探索代码库、生成代码、运行命令并使用外部工具。它可以分担任务、执行更改请求，让你专注于当前工作。现在使用熟悉的工具安装它。

在本课中，你将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 账户完成身份验证。
- 信任研讨会仓库并尝试一次简短对话。
- 通过内置的 GitHub MCP 服务器查找筛选功能议题。

## 场景

团队正使用 AI 智能体处理不断增加的待办事项。Copilot CLI 将这种能力带入许多开发人员日常使用的终端。本课会完成安装和身份验证，为后续研讨会做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 或 Homebrew 安装 Copilot CLI。GitHub Codespaces 已预装 Node.js，因此此处使用 npm。

1. 返回 Codespace 并打开终端。
2. 确认已安装 Node.js，且版本符合要求：

   ```bash
   node --version
   ```

   版本应为 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   终端应显示版本号。

## 使用 GitHub 完成身份验证

首次启动 Copilot CLI 时，系统会提示使用 GitHub 账户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果出现提示，按照设备代码说明进行身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认显示的路径是 Tailspin Toys 仓库，然后选择 **Yes, and remember this folder for future sessions**。

> [!NOTE]
> 在 Codespace 中，你可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 未提示身份验证，就可以继续使用。

## 熟悉环境

在普通 shell 提示符下输入的命令会直接在 Codespace 中运行。启动 Copilot CLI 后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help` 查看当前版本支持的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认一切正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读答复，留意 Copilot 会先探索仓库再回答。
5. 输入 `/mcp list`，确认内置 GitHub MCP 服务器可用。
6. 使用以下提示，让 Copilot 查找筛选功能议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读议题。完成一项快速的首次更改后会再次使用它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都要创建功能分支。

## 使用研讨会快捷方式

Copilot CLI 通常会在使用尚未获准的工具前请求许可。本研讨会会在 Codespace 中使用 `--yolo` 重新启动 CLI，避免反复审批提示，以便专注练习。

> [!CAUTION]
> `--yolo` 会启用完整自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或 VM 等隔离环境中使用，绝不要将其设为日常开发的默认别名。详情参阅[允许和拒绝工具使用][allow-all-warning]。

本研讨会还会使用 `--enable-all-github-mcp-tools` 启用后续课程所需的 GitHub MCP 读写工具，以处理议题和拉取请求。Codespace 限制了对本地计算机的访问，但经身份验证的 GitHub 资源仍是真实资源。发布或合并前请审查更改。

1. 使用 `/exit` 退出 Copilot CLI。
2. 在仓库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再询问一个关于项目的简单问题，确认对话正常工作，然后使用 `/exit` 退出。

Copilot 会自动保存对话。之后更新指令或添加智能体时，使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和后续步骤

恭喜你，已完成本课内容：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 账户完成了身份验证。
- 信任了研讨会仓库并尝试了简短对话。
- 通过内置 GitHub MCP 服务器找到了筛选功能议题。

接下来，开始[第一项聚焦更改][next-lesson]，使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
