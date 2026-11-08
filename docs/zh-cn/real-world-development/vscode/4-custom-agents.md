---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在仓库 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专门指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成相应工作。团队可以将专业知识整理成可复用智能体，例如遵循 [Web 内容无障碍指南 (WCAG)][wcag] 的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体与[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。可以将自定义智能体理解为**工作者**，而技能是它使用的**工具**。

自定义智能体有自己的上下文窗口，并可在工作时协调技能，甚至协调其他智能体。本练习中的无障碍访问自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的写法。与其他 AI 用法一样，应在目标工作环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用众筹平台，不受视觉能力或偏好影响。近期反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景颜色对比度不足。设计团队因此要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。接下来使用自定义智能体生成相关功能。

在本练习中，你将：

- 审查现有的无障碍访问自定义智能体。
- 在 Copilot Chat 代理模式中使用无障碍智能体实现高对比度模式。

## 审查无障碍访问自定义智能体

项目已提供无障碍访问自定义智能体。先审查其内容，了解它会如何引导 Copilot。

返回 Codespace，在终端中从 `main` 创建新分支，以便将无障碍工作与练习 3 的筛选功能拉取请求 (PR) 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续查看后续部分，了解其职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话将遵循该智能体的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动干净的对话。模式和模型选择因练习而异；每个相关步骤都会说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认显示当前智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会以内联方式显示在聊天中，并包含文件路径和差异供你审查。
6. 随着编辑发生，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改仓库，可能会更新 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域的专用 AI 助手。自定义智能体可将团队专业知识和标准整理为可复用配置，引导 Copilot 更有效地完成相应工作。

在本课中，你：

- 了解了自定义智能体的定义方式。
- 在 Copilot Chat 代理模式中使用了自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查其更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
