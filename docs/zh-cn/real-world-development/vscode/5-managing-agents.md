---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot 正在你的计算机上实时工作，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在编辑器中查看每项差异、接受或拒绝单项更改，也可以发送后续提示引导对话，完善或扩展工作。

在本练习中，你将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行中引导智能体，添加浅色模式开关，并保留高对比度模式开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了新需求：他们也希望为觉得默认主题太暗的用户添加浅色模式开关。无需从头开始新对话，可以引导当前会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中以内联差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时滚动聊天记录，查看每次工具调用。你会看到文件读取、文件编辑，以及它请求运行的任何终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确定安全的命令。

5. 等待智能体完成高对比度模式实现。完成后应会看到总结更改的最终消息。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在在同一对话中扩展功能，添加浅色模式开关。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括它修改的组件、所用样式方法和已设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会检查之前的更改，并提出添加浅色模式的方案。
3. 像处理高对比度模式一样，逐项审查新编辑。对每项差异使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不合预期（例如试图重构开关用户界面（UI）），可以使用聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比重新开始更快。可以通过后续提示调整方案（“简化层叠样式表（CSS），使用 CSS 变量而不是重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式集成到现有用户界面（UI）设置面板”）或扩展范围（“还要为两个开关添加键盘快捷键”）。

## 审查工作文件集

提交工作前，快速检查智能体修改的所有内容。

1. 在 Visual Studio Code（VS Code）中打开 **Source Control** 视图。
2. 审查完整的已更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并检查差异。确认自定义智能体要求的无障碍模式已落实，包括无障碍富互联网应用（ARIA）属性、键盘导航、语义化超文本标记语言（HTML）和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你已使用 Copilot Chat 智能体模式监控并引导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留上下文。

本课程中，你：

- 实时监控了智能体的工作。
- 接受或拒绝了单项文件编辑。
- 通过后续提示在会话进行中引导智能体。

在[下一课][next-lesson]中，你将把无障碍功能作为拉取请求发布，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
