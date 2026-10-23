---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可以提供相关指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

在本练习中，你将：

- 了解如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目专属上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）路径不同，本练习不进行修改指令前后的提示对比。Copilot 云智能体会异步处理 GitHub 议题，因此很难实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，可以看到指令文件发挥的作用。

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

## 添加新的存储库标准

下一步将添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后的练习中，云智能体处理议题时就会遵循该规则。

编辑前先创建分支。（云智能体会读取议题目标分支上的指令，但仍建议将更改整洁地提交。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前面提到，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。接下来将编码标准写入指令文件，以改进代码建议。

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

## 提交、推送并合并指令更新

云智能体会读取议题目标分支上的指令。下一练习将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用这些指令。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并完成合并。之后的练习中，云智能体就能读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接操作 `main`，可以跳过分支步骤并直接提交到 `main`。这里创建分支是为了模拟真实项目中的处理方式。

## 总结和后续步骤

你已经：

- 了解了如何通过存储库自定义指令和按路径限定的指令文件，向 Copilot 提供项目专属上下文、编码指南和文档标准。
- 在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。

接下来的练习中，可以在云智能体生成的拉取请求里看到这条标准发挥作用。

接下来，将第一个议题分配给 [Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
