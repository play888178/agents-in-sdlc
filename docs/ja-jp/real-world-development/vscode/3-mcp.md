---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードを使ってフィルター機能を構築しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせ、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外にあるツールを利用できると、さらに強力になります。Copilot がそれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェント モードの内部動作と、コンテキスト、大規模言語モデル (LLM)、MCP サーバーや VS Code 拡張機能が提供するツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントは (リソースを使って) 最新の情報にアクセスし、(ツールを使って) 開発者に代わって操作を実行できます。

これらのツールやリソースは、AI エージェントと外部のツールやサービスの間を仲介する MCP サーバーを通じて利用します。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムの検索や貢献を促す [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトの他の依存関係と同様に扱ってください。使用する前にソース コードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密リソースや操作へのアクセスを許可する際は注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` サーバーがすでに構成されていることを確認します。

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

この `github` エントリはプロジェクト テンプレートに含まれています。そのため、前の演習で Copilot は issue のバックログを読み取ることができました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルでのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。機能のテストに役立ちます。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` のエントリを追加し、次のようにします。

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

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` はローカル サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` フラグを使うと、画面に表示せずブラウザーを実行できます。デスクトップ環境のない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンド テストに Playwright を使っているため、通常はブラウザーがすでにインストールされています。Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試します。

## サーバーを起動して信頼する

VS Code は必要になったときに MCP サーバーを起動します。Copilot がサーバーのツールを初めて使う際に VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で、`playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか確認した場合は、構成をレビューして信頼することを選び、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。開発者がアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って報告できます。会話から離れずに機能の動作を確かめられる、すばやい方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。つまり、エージェントは支援技術と同様に、ボタン、リンク、リスト項目などのラベル付き要素を構造化された情報として扱います。そのため、簡単な機能確認がアクセシビリティの基本的な確認にもなります。

サーバーを接続し、アプリを起動した状態で、Copilot にフィルター機能をテストするよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して、確認した内容を報告します。issue の受け入れ条件と報告を照らし合わせます。期待と異なる点があれば、プルリクエストを開く前に追加で質問するか、コードの修正を依頼します。

> [!NOTE]
> テストを実行するには、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームでレビューできるようプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルを開き、Copilot が行った変更をレビューします。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Source Control パネルで変更を示すスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、複数の方法でプルリクエストを作成できます。ここでは GitHub Copilot に依頼します。関連する issue を見つけ、PR に関連付けて作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択されていることを確認します。これにより、Copilot が GitHub ツールを使えます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作を実行するために必要な場合は、**Continue** を選択します。GitHub ツールを初めて使うときに認証を求められた場合は、画面の指示に従います。
5. Copilot が issue を検索し、該当する issue を見つけて PR を作成することを確認します。
6. Copilot が作成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot に新しいツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- 出荷前に Copilot にブラウザーを使わせ、フィルター機能を手動テストしました。
- ブランチを公開し、フィルター作業のプルリクエストを作成しました。

機能の**テスト**に MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ話題なのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
