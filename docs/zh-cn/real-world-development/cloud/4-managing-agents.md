---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项改善用户体验和添加功能的任务。云智能体可以自主异步工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制台中，可以查看所有拥有未关闭拉取请求（PR）的智能体任务、查看执行的操作，甚至引导正在运行的会话。

在本练习中，你将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体来创建高对比度模式后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，将此功能一并纳入实现是合理的。你将引导智能体，确保它在实现高对比度模式时也添加浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到已分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的应用程序编程接口（API）。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 执行的操作，包括：
   - 检出存储库代码。
   - 创建 Copilot 工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的各种操作。

> [!NOTE]
> 任务列表中的具体步骤会因 Copilot 的工作状态和处理方式而异。

4. 另请查看右侧的拉取请求（PR）面板。此处可查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

查看当前正在执行的任务后，请求 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示已完成设置并开始工作。此时会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中高亮显示“Steer active session while Copilot is working”对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

这同样需要几分钟。等待期间，可以回顾目前已学习和探索的内容。

## 总结和后续步骤

本课程介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在任务控制台中，可以跟踪所有正在进行和已完成的任务、审查正在执行的工作，还可以重定向进行中的任务以调整范围或提供补充指导。

本课程探讨了以下概念：

- 使用任务控制台和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
