---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

[**GitHub Copilot app**][about-copilot-app] 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot 命令行界面（GitHub Copilot CLI）构建，并与 GitHub 原生集成，因此存储库、分支和持续集成（CI）管道均可直接使用。它适用于同时指挥多个智能体的工作流：每个智能体都在隔离的工作区中运行，无需亲自完成所有工作，还可以自动执行重复性任务。现在 Node.js 已安装，项目副本也已准备好，接下来安装应用并连接存储库。

本课将介绍如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正在采用人工智能（AI）智能体来处理不断增加的待办事项。Copilot app 提供了统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何发起有关项目的对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任一付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须启用 **GitHub Copilot app** 策略。

## 安装并配置 GitHub Copilot app

先安装 GitHub Copilot app，再完成身份验证并添加 Tailspin Toys 存储库。应用支持 Windows、macOS 和 Linux。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用启用了数据驻留的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果引导流程提供存储库选择，请选择通过模板创建的 Tailspin Toys 存储库。也可以跳过此步骤，在完成引导流程后再添加项目。
6. 系统提示时选择主题，然后完成引导流程。
7. 在侧边栏的 **Sessions** 下查找存储库。如果已列出，无需再次添加。
8. 如果尚未连接存储库，选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。输入先决条件课程中记下的所有者和存储库名称。
9. 确认存储库显示在侧边栏中。在本课程使用的布局中，`tailspin-toys` 等存储库文件夹归在 **Sessions** 下，每个存储库下方列出相应会话。

## 熟悉工作区

项目连接后，侧边栏中的以下区域可帮助你找到对话、议题、拉取请求和自定义项：

- **New**：启动新的 Copilot 聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成查看和筛选拉取请求、检查持续集成（CI）状态并审查更改。
- **Issues**：浏览和筛选议题，或从议题启动会话。
- **Automations**：保存按计划或按需运行的智能体任务，适用于待办事项、定期维护或其他重复工作。本工作坊会在总结中将其作为后续方向，而不是另一个练习。
- **Customize**：通过模型上下文协议（MCP）服务器、插件、技能和其他组件扩展 Copilot app。稍后会用它配置 Playwright MCP。
- **Chats**：用于提问和集思广益的轻量对话，无需单独创建分支或工作区。开始首次聊天后，此区域才会显示。如果尚未显示，选择 **Sessions** 旁的 **+**，开始首次聊天。
- **Sessions**：归纳存储库文件夹及其智能体会话。选择存储库名称（例如 `tailspin-toys`）打开存储库视图，或选择其下的会话返回对话。每个会话都在独立工作区中运行，因此可以同时启动多个会话，而不会造成更改冲突。添加星级评分时会启动第一个会话。

随着工作坊推进，你将逐步熟悉工作区。

> [!TIP]
> Copilot 也可以提供操作指导，或说明某项功能是否可用。

### 查找模板创建的待办事项

Tailspin Toys 的模板已创建待办事项；查看这些议题可了解后续功能的起点。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择一个议题并阅读详情。每个议题都可以启动智能体会话。完成一项简短的首次更改后，你将从筛选议题开始工作。

> [!NOTE]
> **Issues** 和 **Pull requests** 会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。要查看其他存储库中的工作项，请将相应存储库添加到应用。

## 尝试快速聊天

快速聊天适合用来了解应用本身，无需为一次性问题创建分支或工作树，也不需要启动会话。

1. 在侧边栏中，选择 **Chats** 旁的 **+** 打开新聊天。如果尚未显示 **Chats**，选择 **Sessions** 旁的 **+** 开始首次聊天；之后 **Chats** 会显示出来。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 阅读对话回复。每个会话都在独立的 Git 工作树中运行，因此可以并行启动多个智能体，而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结与后续步骤

你已安装 GitHub Copilot app、连接项目并熟悉工作区。你已经：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中找到模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
