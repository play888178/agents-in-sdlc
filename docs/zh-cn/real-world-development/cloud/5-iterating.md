---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你已与 GitHub Copilot 合作完成多项改善用户体验和添加功能的任务：为代码添加文档、为设计团队构建相关游戏功能以便迭代，以及实现包括高对比度和浅色模式开关在内的无障碍功能。现在一起检查代码更改，并在需要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，引入生成式人工智能（AI）不会改变软件设计和开发与运维（DevOps）的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在启用团队其他成员的审查前，先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会异步且自主地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，对其工作分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境来处理任务。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，云智能体对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，模型上下文协议（MCP）服务器也受此限制。

## 审查生成的文档

先检查 GitHub Copilot 云智能体创建的第一个拉取请求（PR），其中包含为代码添加的文档。通过 GitHub.com 标准 PR 界面完成检查。

> [!NOTE]
> 查看拉取请求（PR）时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器调用。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更详细标题的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。此时请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息一下或回顾已学内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TypeScript 文档注释（TSDoc）和其他文档。具体更改会有所不同。

    查看更改时，检查 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自练习 2 开始时查看的自定义指令文件。

6. 确认更改看起来正常后，返回 **Conversation** 选项卡并向下滚动。
7. 检查是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查会通过。可能需要几分钟才能完成。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话来处理请求。由于结果具有非确定性，无法给出固定的提示文本。可以要求 Copilot：

- 在每个代码文件开头添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TypeScript 文档注释（TSDoc）。
- 创建一个说明 Astro 应用程序结构的 README。

1. 在评论中提及 **@copilot**，并请求它修改生成的文档。可以使用上述建议之一，也可以提出其他文档改进请求。
2. 选择 **View Session**，观察 Copilot 完成工作。注意 Copilot 会启动新会话来更新内容。
3. 选择 **Back to pull request**，即可返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到结果符合预期。满意后，可将 PR 从草稿状态标记为准备审查，并合并到主分支。

![将 PR 标记为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在检查 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 或更详细标题的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 确认更改看起来正常后，返回 **Conversation** 选项卡并向下滚动。
6. 检查是否有等待批准的工作流指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查会通过。可能需要几分钟才能完成。
9. **可选：** 也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。和之前一样，可以通过添加评论请求 Copilot 云智能体迭代更新。例如，可以请求调整显示的相关游戏数量，或者添加文件注释头和 TSDoc 文档注释（注意，分配该任务时还没有更新自定义指令）。在 **Conversation** 选项卡中添加评论后，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后检查使用无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制台中要求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 或更详细标题的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求中会显示 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 检查实现，特别留意：
   - 用于切换模式的用户界面（UI）组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改看起来正常后，返回 **Conversation** 选项卡并向下滚动。
7. 检查是否有等待批准的工作流指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会在拉取请求的检查部分排队运行。如果一切正常，单体 Astro 应用程序的项目检查会通过。可能需要几分钟才能完成。
10. **可选：** 可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后设置仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进点，可以像处理之前的拉取请求（PR）一样在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委派任务

云智能体适合处理真实待办事项，让你能专注于其他工作。可以针对自己的存储库创建更多议题并分配给 Copilot，练习这种工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列课程中，你：

- **检查了存储库自带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行任务。
- **创建并使用了自定义智能体**，实现高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制台**，监控并引导正在执行的无障碍任务。
- **审查并迭代了云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成云智能体学习路径。其他学习路径可以补充本路径的实践内容，继续探索不同工作环境：

- 🖥️ **[VS Code 路径](../../vscode/)** — 在集成开发环境（IDE）中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[命令行界面（CLI）路径](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义质量保证（QA）智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接功能。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
