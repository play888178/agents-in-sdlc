---
title: "练习 3 - 使用 Playwright 模型上下文协议（MCP）服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。打开拉取请求前，应在浏览器中确认功能确实有效。本练习中，你将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，而不是自己逐项操作；之后发布分支并创建拉取请求（PR）。

在本练习中，你将：

- 了解模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用该服务器在浏览器中手动测试筛选功能。
- 发布分支并为筛选工作创建拉取请求。

## 什么是模型上下文协议（MCP）？

智能体模式可以使用编辑器之外的工具，从而发挥更大作用。Copilot 通过模型上下文协议（MCP）实现这一点。MCP 是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，展示其如何与上下文、大语言模型和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议（MCP）](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能（AI）智能体提供与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，通过资源获取最新信息，并使用工具代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务（例如现有应用程序编程接口（API）或 npm 包等本地工具）之间的桥梁，并负责管理通信。每个 MCP 服务器都会提供一组 AI 智能体可以使用的工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的应用程序编程接口（API）。AI 智能体可以使用它创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以用它浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可以提供不同工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，方便开发者发现并参与生态建设。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细检查其源代码、验证发布者并考虑安全影响。只使用可信的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应能看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含这个 `github` 条目，因此上一练习中 Copilot 才能读取议题待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 会通过 HTTP 连接该服务器，并在 Copilot 首次使用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好可以用来测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器；codespace 没有可显示浏览器窗口的桌面，因此需要此设置。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要使用工具时启动服务器，并提示你信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了可操作的真实浏览器。智能体可以打开页面、浏览、应用筛选条件并读取结果，然后汇报观察到的情况，无需你亲自操作。这是无需离开对话即可快速确认功能符合预期的方式。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]，而不是截图。智能体可以依据带标签的结构化元素（如按钮、链接和列表项）进行判断，这与辅助技术的工作方式相同。因此，快速功能检查也能顺便进行基本的无障碍检查。

连接服务器并启动应用后，让 Copilot 测试刚实现的筛选功能。

1. 发送以下提示：

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

2. 阅读 Copilot 的报告，并根据议题中的验收标准检查结果。
3. 如果发现问题，提出后续问题，或要求 Copilot 在打开拉取请求之前修复代码。

> [!NOTE]
> 测试时，应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以打开拉取请求（PR），供团队审查。先发布 `filtering-vscode` 分支。

1. 在 codespace 中打开 **Source Control** 面板，审查 Copilot 做出的更改。
2. 选择 **+** 暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面（CLI）。由于已在使用 GitHub Copilot，可以让它代为创建 PR。它可以找到相关议题并建立关联。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示请求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，可能会提示登录 GitHub；按照提示授权即可。
5. 注意 Copilot 如何搜索议题、找到正确的议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议（MCP）以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用该服务器在浏览器中手动测试筛选功能，然后再发布。
- 发布分支并为筛选工作创建拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，创建一个[自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
