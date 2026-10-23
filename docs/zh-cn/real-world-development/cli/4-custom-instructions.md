---
title: "课程 4 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

在 GitHub Copilot CLI（命令行界面）中使用生成式人工智能（AI）时，上下文至关重要。任务需要以特定方式完成时，应让 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明希望生成**什么**代码，也说明代码应当**如何**组织。现在筛选功能已完成，你将探索 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课程中，你将：

- 探索存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 和其他优秀的软件团队一样，有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TypeScript 文档注释（TSDoc）/JavaScript 文档注释（JSDoc）说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 契约，相关代码更改后也应及时更新注释。
- 应保留现有格式和 lint 指导。

使用指令文件可以确保 Copilot 获取正确的信息，并按照上述实践完成任务。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。这项强大的功能可以引导 Copilot 提供更相关的建议和代码片段。你可以为整个存储库创建指令，也可以为特定文件类型或任务创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：单个指令文件，会随该存储库中的**每个**请求发送给 Copilot。它应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 探索此项目中的自定义指令文件

入门项目已经包含一组指令文件。更改前，先看看当前内容，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，注意项目的简要介绍和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览文件。注意其中有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了用于确定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他指令文件和项目现有文件的链接。这样可以将大型指令集拆分为更小且可复用的文件，并为 Copilot 提供可参考的示例。

## 更新指令文件以符合团队指导

现有文件是不错的起点，但仍有缺漏。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 中包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常位于第 35 行附近。
2. 在该部分末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时使用新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中，恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，阅读已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从指令文件获取上下文，并在功能中应用了新标准。具体来说，你：

- 探索了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，自定义并运行可复用的 `quality-checks` 技能，确保 lint 和测试能够一致执行。

## 资源

- [向 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
