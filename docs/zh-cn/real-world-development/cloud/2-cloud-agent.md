---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成任务后，它会创建一个草稿拉取请求（PR），供开发人员审查。这样可以将繁琐工作交给智能体，提升开发效率，让开发人员专注于更重要的工作。

本练习将探索 Copilot 云智能体如何：

- 自定义代码生成环境。
- 确保操作安全。
- 处理范围明确的议题。
- 接收议题分配。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初开发网站的承包商没有留下合适的文档——实际上，代码完全缺少文档。团队希望首先为应用程序中的所有导出函数添加 TypeScript 文档注释（TSDoc）。

此外，设计团队准备改善游戏发现体验。他们希望在每款游戏的详情页显示相关游戏，即同一类别中的其他游戏。暂时不需要精致的实现，只要可以用于用户体验验收测试即可。这项工作当前被阻塞，但优先级较高的议题还未完成。

这两项工作都可能因优先级调整而被搁置，因此适合交给 Copilot 云智能体。云智能体可以异步处理任务，让开发人员先做其他工作，再回来审查 Copilot 的结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。与人类开发人员协作一样，可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建一个用于跟踪进度的草稿拉取请求（PR），设置环境并开始处理任务。任务进行中或完成后，你都可以查看 Copilot 会话。提出的解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，不能免去思考过程。即便看似简单的操作，深入分析后也会发现其复杂性。因此，[为 Copilot 云智能体设定任务范围时要谨慎][cloud-agent-best-practices]。应将它视为人工智能（AI）结对编程助手：分阶段工作、学习、试验并根据情况调整。生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已经完成过自定义指令练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。将工作分配给 Copilot 云智能体前，只读查看存储库中现有的指令文件，以便稍后观察其影响。

在 GitHub 网页界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将指令限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述指令文件，稍后的练习还会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保开发环境状态正常。将任务分配给 Copilot 时也一样，它会像软件工程师（SWE）一样处理任务。

云智能体通过 [GitHub Actions][github-actions] 设置工作环境。可以创建[专用设置工作流][setup-workflow]，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行，以便安装所需的开发工具和依赖项。本实验已预先配置好该工作流，便于顺利完成练习。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用程序所需的迁移和预置 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为这些步骤实际需要的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆，步骤完成后 Copilot 会自动克隆存储库。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置：Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并预置本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流类似，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 注意 `workflow_dispatch` 触发条件，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以先验证工作流是否成功，而不必等 Copilot 运行。

## 添加文档

尽管大家都知道文档的重要性，但许多项目中的文档要么过时，要么完全缺失。这类技术债务常常得不到处理，降低工作效率，也增加维护代码库或引入新开发人员的难度。Copilot 擅长生成文档，因此这是适合分配给 Copilot 云智能体的任务。它会在后台生成所需文档。之后的练习中，你会回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），其标题应类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新的 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 审查列表和它准备完成的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解项目状态、暂停思考和确定方案，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让开发人员能够专注于其他工作。接下来将利用这一点，再分配一项任务给 Copilot 云智能体，同时开始为应用程序编写代码并添加功能。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot 云智能体的一项重要优势是可以拆分工作：你专注于一组任务时，它可以处理另一组任务。即使为设计团队添加相关游戏功能不会花费太多时间，也可以将时间用于其他工作。现在将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题中的第一条评论旁边会出现一组 👀，表示 Copilot 已开始工作。

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理你的第二项请求。Copilot 云智能体的工作方式类似软件工程师，你无需持续监控它；任务完成后再审查结果即可。现在开始了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课程介绍了 [GitHub Copilot 云智能体][copilot-agents]。借助云智能体，你可以将议题分配给 Copilot，让它异步处理。它可以解决技术债务、创建新功能，或协助将代码迁移到其他框架。

本课程探讨了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议（MCP）服务器][cloud-agent-mcp]，并支持自定义指令；前面的模块已经介绍过这些内容。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
