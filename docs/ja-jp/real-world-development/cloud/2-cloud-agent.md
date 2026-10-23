---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなど、さまざまな課題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に行います。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

この演習では、Copilot cloud agent について次のことを確認します。

- コード生成環境をカスタマイズする方法
- 操作が安全に行われるようにする方法
- 範囲を明確にした issue の重要性
- issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。サイトの初期バージョンを作成した開発業者はドキュメントを十分に整備していません。実際、ドキュメントはほとんどありません。最初の作業として、アプリケーションのエクスポートされた関数すべてに TSDoc ドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。ゲーム詳細ページに同じカテゴリの別のタイトルを表示し、利用者が閲覧を続けられるようにします。現時点では、受け入れテストで UX を確認できる程度の実装で十分です。この作業は現在ブロッカーですが、優先度の高い issue がほかにもあります。

どちらも優先順位が下がりやすい作業ですが、Copilot cloud agent に割り当てるのに適しています。cloud agent が非同期で作業する間、開発者は別のタスクに集中し、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドで作業できます。[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエスト (PR) を作成し、環境をセットアップして作業を始めます。実行中または完了後にセッションの内容を確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく調べると複雑な場合があります。[Copilot cloud agent に依頼する作業の範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響について学びました。カスタム指示の演習を終えていれば `.github/copilot-instructions.md` が機能する様子を確認済みです。まだの場合は、この機会に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリにある指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で、次の手順に沿って指示ファイルを確認します。

1. `.github/copilot-instructions.md` を開きます。**Code standards** セクションを確認し、特に TypeScript 規約と TSDoc ドキュメント コメントの要件に注目します。
2. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、プルリクエストに TSDoc コメント、TypeScript 規約、ファイル先頭のコメントが反映されているか確認します。これらの指示ファイルに由来する内容は、後の演習で PR をレビューする際にも扱います。

## Copilot cloud agent の開発環境を設定する

コードを作成するときは、誰が作業する場合でも、必要な環境や状態を整えるセットアップ スクリプトが必要です。ソフトウェア エンジニア (SWE) のように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境に [GitHub Actions][github-actions] を使います。作業開始前に必要な開発ツールや依存関係を用意するには、`.github/workflows/copilot-setup-steps.yml` に[専用のセットアップ ワークフロー][setup-workflow]を作成します。このラボでは、手順を進めやすくするため、ワークフローをあらかじめ構成しています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシード済みの SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 各手順に必要な最小限の権限を設定します。Copilot の操作には専用トークンが付与されます。
    permissions:
      # 依存関係のインストールなどでセットアップ中にリポジトリをクローンする場合は、contents: read 権限が必要です。クローンしない場合は、手順完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションしてシードし、ビルドやテスト用データを準備します。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様ですが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブがあります。このジョブは Copilot がプルリクエストの作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が動かすのを待たずに、ワークフローが正常に動作するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は広く知られていますが、多くのプロジェクトでは情報が古かったり、ドキュメントが不足していたりします。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成にも役立つため、この作業を Copilot cloud agent に割り当てます。後の演習で生成されたドキュメントをレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot に issue を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかる場合があります。Copilot は先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、最適な進め方を判断しています。

13. todo リストと実行予定の作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するには、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて判断し、コードを作成する過程を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的は、作業を非同期で実行し、開発者が他のタスクに集中できるようにすることです。この利点を活用するため、もう 1 つ作業を割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。開発者が別のタスクに集中する間に、エージェントが別の作業を進められます。デザイン チーム向けの関連ゲームの追加にそれほど時間がかからないとしても、その時間を他の作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. 空のテンプレートを使うため **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

  ![Copilot に issue を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューします。続いて、カスタム エージェントの作成と使用に進みましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当て、非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次のことを確認しました。

- コード生成環境のカスタマイズ
- 操作が安全に行われるようにすること
- 範囲を明確にした issue の重要性
- issue を Copilot に割り当てること

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示も参照します。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
