---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

在本课中，你将：

- 探索自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选功能议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，使用一致的流程检查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用角色。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

已使用的自定义功能各有用途：仓库指令描述团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力检查需求并报告发现。QA 配置文件不会取代这些工具，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建一个可复用的 QA 配置文件，其中定义 QA 执行的检查及其必须遵循的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义智能体。它应根据议题和已商定的需求检查功能，遵循仓库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求同意，也不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新智能体前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免它在你只想验证功能时进行不完整检查或过度修改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但可以作为清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件缺少上述职责或边界，请先要求默认智能体修改，再继续。

## 根据议题运行 QA

创建配置文件后，Copilot CLI 会在对话启动时加载项目智能体。恢复同一个筛选功能对话，再选择 QA，以便它使用已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中，但仍可使用上述命令启用。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   根据议题和计划中确定的决定审查筛选功能。它是否已准备好创建 PR？
   ```

3. 等待 QA 智能体开始工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专业角色，并审查了它的工作。在本课中，你：

- 探索了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选功能议题审查了它的发现。

现在已准备好实现、技能更新、QA 配置文件、测试和验证报告。接下来将它们汇总到[功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
