---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保它能访问这些内容。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解应如何完成。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过仓库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的全仓库标准。

> [!NOTE]
> 与 VS Code 和命令行界面 (CLI) 工作环境不同，这里不会运行“前后对比”提示。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，可以看到指令文件带来的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件顶部都应有一段说明文件用途的注释。

使用指令文件可以确保 Copilot 获得正确的信息，并按照这些实践要求完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码约定、库以及希望包含的注释类型。既可以为整个仓库创建指令，也可以为特定文件类型创建指令，提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：针对仓库的**每个**请求都会发送给 Copilot 的单一指令文件。内容应为适用于大多数聊天或命令行界面 (CLI) 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体生成代码时，都会使用仓库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 `AGENTS.md` 和 `CLAUDE.md`。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会范围，但示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门指导。
- 使用自然语言，保持指导清晰，并提供正反示例。

指令文件没有唯一正确的写法，AI 的使用方式也没有唯一正确答案。通过实验找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。本项目提供了多种任务示例，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同工作环境的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前工作环境的课程会在相关处说明。
>
> 需要模板或起点时，可以探索汇集指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义指令文件

花些时间阅读仓库附带的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置一个相对于仓库根目录的 glob；这里任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看其中专门针对本项目单元测试的指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供示例。（其中路径相对于指令文件，而非仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的仓库标准

接下来只需编辑一处：添加全项目规则，要求代码使用 TSDoc 文档注释，并在文件顶部添加注释头。云智能体之后处理分配给它的议题时会读取这些指令。

先创建工作分支。云智能体会读取议题目标分支中的指令，但仍应以常规方式提交更改。

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但还没有代码内文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前所述，指令文件可以放在仓库级别的 `.github/copilot-instructions.md` 中提供全局指导，也可以放在 `*.instructions.md` 中指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目标准，适合放在仓库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将指令更新合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并。这样云智能体在下一练习处理议题时就能读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上操作，也可以跳过分支步骤并直接提交。本练习采用分支，是为了模拟真实项目的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目的指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加了新的仓库级标准。后续练习中可以观察云智能体生成的拉取请求是否遵循了该标准。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
