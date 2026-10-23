---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI の Playwright MCP を確認または構成し、ブラウザーでフィルター機能を調べます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発作業ではありません。データや外部サービスを扱い、Copilot に追加の自動化機能を提供することもあります。そのために使うのが MCP サーバーです。MCP サーバーを利用すると、Copilot は CLI に組み込まれた機能を超えて、さらに多くのツールやサービスを使えます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどう利用するかを学びます。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を調べます。

## シナリオ

ユニット テストやエンドツーエンド テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot が利用者と同じように Web サイトを使えるようにすると、変更の自動化をさらに進め、期待どおりに動作する確信を高められます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うことで、AI エージェントは外部のツールやサービスとリアルタイムに通信し、最新の情報にアクセスして、代わりに操作を実行できます。

ツールやリソースは、AI エージェントと外部サービスの橋渡しをする MCP サーバーを通じて利用します。各 MCP サーバーは、エージェントがアクセスできるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムの検索や貢献を促す [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同じように扱ってください。使用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、追加して Copilot が利用者のように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI に Playwright MCP がすでに構成されている場合は、セットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を調べるよう Copilot に依頼する

これまでは、機能が期待どおりに動くことを手動で確認しました。今度は、Copilot から利用できる Playwright MCP サーバーを使って同じことを行います。

1. 次のプロンプトで、Playwright MCP サーバーを使って機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動的に判断して使うため、明示的に指定する必要はありません。ただし、使うべきツールが分かっている場合は、方向を示すことで結果の一貫性が高まり、トークンも節約できます。

2. Copilot がブラウザーで機能を確認するために行う操作を確認します。
3. 報告を読み、すべて期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、結果を報告します。

## まとめと次のステップ

Playwright MCP サーバーを使って、Copilot CLI から実際のブラウザーで機能を調べました。次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- 未構成の場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を調べました。

次は、スキルとブラウザー ツールを組み合わせる[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
