---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 应了解某些背景信息，就需要确保它能访问这些内容。[指令文件][instruction-files]是提供代码生成指导的重要方式。

在本练习中，你将：

- 探索项目专属上下文、编码准则和文档标准如何通过仓库自定义指令及按路径限定范围的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加一项新的仓库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能数据切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的标准。Copilot 具有非确定性。本练习的重点是观察更新指令后行为的*趋势*，而不是要求输出逐字符一致。

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

- `.github/copilot-instructions.md`：针对仓库的**每个**请求都会发送给 Copilot 的单一文件。内容应为适用于大多数聊天或命令行界面 (CLI) 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
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

阅读仓库附带的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导；这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看其中针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段设置一个相对于仓库根目录的 glob；这里任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看其中专门针对本项目单元测试的指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供示例。（其中路径相对于指令文件，而非仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

按照最佳实践，为本次更改创建分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令*之前*使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，以启动干净的对话。模式和模型选择因练习而异；每个相关步骤都会说明。

为观察自定义指令的影响，先在当前指令生效时发送提示。稍后更新指令后，再发送相同提示进行比较。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放置的位置。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以允许 Copilot 修改文件。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改 helper 文件及其测试。
6. 注意生成的 helper 是带类型的函数，第一个参数接收 `db` 客户端，并返回带类型的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求也可能添加文档注释。如果发生这种情况也没关系，本练习重点仍是观察添加指令后*一致性*的变化。

## 添加新的仓库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在补充仓库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。注意其中记录了项目编码标准，但尚无代码内文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在仓库级别的 `.github/copilot-instructions.md` 中提供全局指导，也可以放在 `*.instructions.md` 中指导特定语言、文件类型或任务。刚添加的文档注释规则属于项目标准，适合放在仓库级文件中。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在已引导 Copilot 遵循新的项目标准，并将其应用到下一练习将继续使用的代码中。

## 提交指令并推送分支

指令文件与仓库中的其他资源一样，使用常规源代码管理流程。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令和 helper 更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为 helper 基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办事项中的文档标准议题。Copilot 可以通过已连接的 GitHub 工具查找议题、创建并关闭议题，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上执行操作。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍此连接的工作方式（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件有关的议题，并从当前分支创建 PR，说明指令更新及 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示要求 Copilot 合并 PR 并将本地分支切回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，检查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，并观察现有指令下 Copilot 生成的内容。
- 向 `.github/copilot-instructions.md` 添加新的仓库级标准。
- 再次运行相同提示，观察建议的代码如何采用新标准。
- 将指令更新和 helper 基础提交到 `main`，供下一练习使用。

接下来，在[代理模式][next-lesson]中运用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot——指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
