---
title: "课程 6 - 使用 Playwright 模型上下文协议（MCP）验证功能"
description: "检查或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码不只是输入代码。我们还需要处理数据、外部服务，并为 Copilot 提供更多自动化能力。MCP 服务器可以扩展 GitHub Copilot CLI（命令行界面）的内置功能，为它提供更多工具和服务。

在本课程中，你将：

- 了解模型上下文协议（MCP）是什么，以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面（UI）更新还需要实际与界面交互。让 Copilot 像用户一样使用正在开发的网站，可以进一步自动化验证流程，并提高对更新符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]让人工智能（AI）智能体能够与外部工具和服务通信。借助 MCP，AI 智能体可以实时访问外部工具和服务，获取最新信息并代表你执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都会提供一组 AI 智能体可以使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的应用程序编程接口（API）。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 托管了 [MCP 注册表][mcp-registry]，以便开发者发现并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请检查源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未提供 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤并继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

你已手动确认功能符合预期。现在使用 Copilot 中提供的 Playwright MCP 服务器进行同样的验证。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，因此不一定需要明确指定。不过，既然你知道应使用哪个工具，指出这一点有助于获得更稳定的结果，也能节省一些令牌。

2. 观察 Copilot 列出的浏览器操作步骤。
3. 阅读报告，确认所有行为均符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你已通过 Copilot CLI 使用 Playwright MCP 服务器，在真实浏览器中探索功能。回顾一下，你已经：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未配置时添加了该服务器。
- 让智能体操作浏览器并探索筛选功能。

接下来，创建一个[质量保证（QA）自定义智能体][next-lesson]，将技能和浏览器工具结合起来，构建专门的质量检查流程。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
