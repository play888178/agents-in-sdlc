---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的人工智能（AI）助手。通过在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，可以向 Copilot 提供重点明确的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业经验编码为可复用智能体，例如执行 [Web 内容无障碍指南（WCAG）][wcag]合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含至少带有 `name` 和 `description` 的 YAML（YAML Ain't Markup Language）frontmatter，后接定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]在概念上有一些重叠。两者主要使用 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是执行者，技能是工具**。

自定义智能体拥有自己的上下文窗口，并且可以协调技能（甚至其他智能体）完成工作。在本练习中，无障碍自定义智能体会根据无障碍指南审查并更新网站；在执行过程中，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景颜色的对比度不足，一些用户难以阅读当前的暗色主题。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义智能体生成所需功能。

在本练习中，你将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 检查无障碍自定义智能体

项目已经为无障碍工作创建了自定义智能体。先检查其内容，了解它将如何引导 Copilot。

先从 `main` 创建无障碍功能分支，以便与练习 3 中的筛选拉取请求（PR）分开处理。

1. 返回 codespace，在终端中运行以下命令：

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. 打开 `.github/agents/accessibility.md`。
3. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

4. 接下来浏览并检查以下部分：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 下定义的所有自定义智能体。选择自定义智能体后，可以在该智能体的指令和工具范围内进行聊天。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的新对话。模式和模型选择因练习而异；需要时会在相应步骤中说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单（默认情况下会显示当前智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，包括 Astro 组件、CSS 和相关测试。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义智能体可以将团队的专业知识和标准封装为可复用的智能体，从而引导 Copilot 更有效地完成特定类型的工作。

本练习中，你已经：

- 检查了现有的无障碍自定义智能体。
- 在 Copilot Chat 智能体模式中使用了无障碍智能体，实现高对比度模式。

接下来，[监控并引导智能体的工作][next-lesson]，在更改进行时审查它们，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在集成开发环境（IDE）中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
