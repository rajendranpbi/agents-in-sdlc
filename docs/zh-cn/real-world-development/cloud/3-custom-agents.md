---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在仓库的 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专门指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地处理相应工作。团队可以将专业知识整理成可复用的智能体，例如遵循 [Web 内容无障碍指南 (WCAG)][wcag] 的无障碍访问智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义智能体行为、专业知识和指令的 Markdown 提示。

### 自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。可以将自定义智能体理解为**工作者**，而技能是它使用的**工具**。

自定义智能体有自己的上下文窗口，并可在工作时协调技能，甚至协调其他智能体。本实验中的无障碍访问自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的写法。与其他 AI 用法一样，应在目标工作环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍如何：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用众筹平台，不受视觉能力或偏好影响。近期反馈指出，一些用户觉得当前深色主题难以阅读，因为文本与背景颜色对比度不足。设计团队因此要求实现可由用户开启或关闭的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。接下来使用自定义智能体生成相关功能。

## 审查无障碍访问自定义智能体

项目已提供无障碍访问自定义智能体。先审查其内容，了解它会如何引导 Copilot。

1. 返回 Codespace，然后打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 继续查看后续部分，了解其中介绍的职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以在此将任务分配给 Copilot 云智能体、监控任务并引导会话。现在分配一项任务，让 Copilot 实现高对比度模式。

1. 打开仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 在 **Title** 中输入 `Add high contrast mode to website`。
6. 在 **Description** 中输入：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![云智能体分配界面截图，突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域的专用 AI 助手。自定义智能体可将团队专业知识和标准整理为可复用配置，引导 Copilot 更有效地完成相应工作。

在本课中，你：

- 了解了自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来前往任务控制中心，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
