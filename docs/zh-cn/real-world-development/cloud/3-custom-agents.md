---
title: "练习 3 - 自定义智能体"
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

本练习将探索如何：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，由于文本与背景颜色的对比度不足，一些用户难以阅读当前的暗色主题。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义智能体生成所需功能。

## 检查无障碍自定义智能体

项目已经为无障碍工作创建了自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 codespace，然后检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 接下来浏览并检查以下部分：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言（HTML）、层叠样式表（CSS）和 JavaScript 示例。
   - 常见问题和错误列表。

## 创建并分配议题

任务控制台是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot 云智能体、监控任务、重定向任务并提供补充指导。现在将创建高对比度模式的任务分配给 Copilot。

1. 打开存储库。
2. 选择 **issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配界面截图，其中高亮显示了自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课程介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。自定义智能体可以将团队的专业知识和标准封装为可复用的智能体，从而引导 Copilot 更有效地完成特定类型的工作。

本课程探讨了以下概念：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来，从任务控制台[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
