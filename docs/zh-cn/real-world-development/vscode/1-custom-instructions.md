---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目专属上下文、编码指南和文档标准。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选代码切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合设定的标准。Copilot 具有概率性。本练习旨在观察更新指令后行为的*整体变化趋势*，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的软件团队一样，有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用暗色模式并具有现代感。
- 代码中应使用 TypeScript 文档注释（TSDoc）。
- 每个文件顶部都应添加一段说明文件用途的注释。

使用指令文件可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项强大的功能可以引导 Copilot 提供更相关的建议和代码片段。你可以为整个存储库创建指令，也可以为特定文件类型或任务创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：单个指令文件，会随该存储库中的**每个**请求发送给 Copilot。它应包含适用于大多数聊天或命令行界面（CLI）请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建的指令文件。例如，可以为 TypeScript 或 Astro 编写指南，也可以针对创建用户界面（UI）组件或单元测试的任务编写指南。

> [!NOTE]
> 在集成开发环境（IDE）中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

本工作坊无法完整介绍如何创建指令文件，但示例项目展示了一种常见做法。总体而言：

- `copilot-instructions.md` 应专注于项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件提供文件类型（如单元测试、Astro 组件、数据层）或特定任务的指导。
- 使用自然语言。保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建指令文件或使用 AI 并没有唯一的方法。通过实际尝试，找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该有一套完善的指令文件。浏览此项目的指令文件时，可能会看到许多不同任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应界面的课程会在相关位置说明操作方法。
>
> 想查找模板或起点？可以探索 [awesome-copilot][awesome-copilot]，其中包含许多指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

花一点时间阅读存储库自带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，注意项目简要介绍以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还可以看到嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意这里有针对 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意文件顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用来确定指令适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 查看此项目中有关创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方法可以将大型指令集拆分为更小且可复用的文件，也可以为 Copilot 提供代码示例。路径是相对于指令文件所在位置的。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的新对话。模式和模型选择因练习而异；需要时会在相应步骤中说明。

先使用当前指令文件发送提示，之后更新指令并再次发送相同提示，观察差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放置的位置。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以应用文件更改。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试文件。
6. 注意，建议的辅助函数是一个类型化函数，它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意，建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使没有明确要求，它也可能添加文档注释。这没有问题；本练习的重点是观察更新指令后行为的一致性变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入指令文件，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常位于第 27 行附近。注意其中记录了项目编码标准，但尚未包含代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指令，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务编写指令。刚添加的文档注释规则是项目级标准，因此适合放在存储库级文件中。

## 再次发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次单击 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在建议的文件开头会出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，现在建议的函数中会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到实际代码中。下一练习将在此基础上继续构建。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，使用相同的源代码管理方式。现在将指令更新和分支推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改都已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他支持文件更新（例如测试），也将它们添加进来。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在为指令标准更新创建拉取请求，并关联待办事项中的议题。也可以手动完成，但 Copilot 可以使用项目已连接的 GitHub 工具代劳。让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。之后的练习会介绍该连接如何工作，也就是模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选择了 **Agent**。
3. 请求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，同时说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，检查命令，并按需选择 **Allow**。
6. 创建 PR 后，使用以下提示让 Copilot 合并 PR 并将本地分支切回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，检查命令并按需选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成了以下操作：

- 探索了存储库自定义指令和按路径限定的指令文件如何为 Copilot 提供项目专属上下文、编码指南和文档标准。
- 发送代码生成提示，观察 Copilot 如何使用*现有*指令生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次发送相同提示，观察建议的代码如何遵循新标准。
- 将指令更新和辅助函数基础更改提交到 `main`，供下一练习使用。

接下来，在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
