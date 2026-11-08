---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索仓库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明想要什么代码，也说明代码应如何组织。你已构建筛选功能，现在将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，你将：

- 探索仓库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 了解指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 Lint 指南。

通过指令文件，可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定首选的编码约定、库，甚至希望代码包含的注释类型。既可以为整个仓库创建指令，也可以针对特定文件类型创建指令，为任务提供上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：单个指令文件，会随针对该仓库的**每个**请求发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因工作环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索本项目中的自定义指令文件

入门项目已包含一组指令文件。先探索现有文件，再进行更改，以便了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，了解项目简介及编码指导。这些指令适用于 Copilot 在此仓库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这可以将大型指令集拆分成更小、可复用的文件，并为 Copilot 提供参考示例。

## 更新指令以符合团队标准

现有文件已打下良好基础，但仍有缺项。现在修改核心 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 将以下内容添加为该部分最后一条项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载仓库指令。保存更新后的指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请更新你生成的代码，使其符合这些指导。
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新的标准应用于功能。在本课中，你：

- 探索了仓库指令和按路径限定范围的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 了解了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 Lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
