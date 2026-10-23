---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は関連するファイルを探し、変更を加え、テストを実行し、問題をデバッグし、見落としたファイルを見つけて再度更新する、という作業を繰り返します。

このような作業に役立つのが Copilot エージェント モードです。

Copilot エージェント モードは統合開発環境 (IDE) でより自律的に動作します。開発者と同様に、まずプロジェクトの構成を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかれば自動的に修正します。エージェント モードを使って、サイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内でタスクを実行し、すばやいフィードバックと対話を可能にします。cloud agent はチームの開発者のように非同期で動作し、issue とプルリクエストを通じて連携します。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを特定して、複数ファイルにまたがる変更を行う方法
- GitHub Copilot エージェント モードで UI とデータ層をまたぐ新機能を実装する方法
- コードベースにマージする前に、Copilot が生成した変更とテストをレビューする方法

## シナリオ

ゲーム一覧が増えてきたため、利用者がカテゴリや発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は、Copilot エージェント モードを使い、残りのデータ層、ユーザー インターフェイス (UI)、テストの作業を完了します。

## Tailspin Toys Web サイトを実行する

変更を加える前に、Tailspin Toys Web サイトの現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示します。Web サイトは単一の Astro アプリで、ビルド時にページを静的なハイパーテキスト マークアップ言語 (HTML) として生成します。ページは frontmatter から Drizzle オブジェクト リレーショナル マッパー (ORM) を通じてローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準のドキュメント オブジェクト モデル (DOM) API を使った小さな Astro `<script>` で実装します。

### Web サイトを起動する

Web サイトを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後、`main` に戻っていることを確認して codespace に戻ります。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を含むメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザーを開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> codespace のターミナルで localhost の URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行している codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な設定は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認します。テンプレートからプロジェクトを設定すると、GitHub issue のバックログが自動的に作成されています。GitHub Copilot に項目を表示するよう依頼します。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、ファイルをまたいで自律的に計画と実装を行い、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。ワークショップを作成する際に Claude Sonnet 4.5 を使用したため、手順に含めています。提案されるコードを比較的一貫させ、スムーズに体験できることを意図しています。ただし、LLM は確率的に動作するため、ワークショップの記載とは異なる提案が生成される場合があります。これは通常の動作です。

> [!NOTE]
> 確率的に動作するため、Copilot が異なる MCP コマンドを使う場合があります。それでもタスクは完了できるはずです。

3. Copilot に次のプロンプトを送り、issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot は、利用者にとって特に有用だと判断した項目を優先して表示します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを提供する指示ファイルを確認します。Web サイトに機能を追加する方法が記載された[ユーザー インターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関するガイダンスを確認します。内容には次の項目が含まれます。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類に応じた指示ファイルへのリンク

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準にします。指示の再利用、複雑な指示の分割、例やテンプレートの提示に役立ちます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つを変更する必要があります。

- データ層 (`src/lib/`) にフィルター ロジックを追加または改善する
- フィルターの動作をテストするテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

コードベースにマージする前にテストを実行し、成功することも必要です。Copilot エージェント モードにこれらの作業を依頼します。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. 関連するバックログ issue の要件に沿って機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを探して変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調査する様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによって、数分かかる場合があります。その間、エディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業が完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot の指示に従って **Continue** を選択し、テストを実行します。

   ![Copilot Chat ペインで、テストを実行してよいか確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコードの生成とテストを反復し、エラーがないと判断するまで続けます。

   ![Copilot エージェント モードの完了したチャット セッションのスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使い、Tailspin Toys Web サイトに新機能を追加する方法を確認しました。次のことを学びました。

- GitHub Copilot エージェント モードで UI とデータ層をまたぐ新機能を実装する方法
- Copilot エージェント モードがプロジェクトを調査し、関連ファイルを特定して、複数ファイルにまたがる変更を行う方法
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法

次は、[Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成しましょう。

### 追加の探索演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使って、Copilot にサイトのページ分割を依頼してみましょう。実装を考える際には、次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページ サイズを利用者が選べるようにするか、固定値にするかなど、ページ分割の方法を決めます。
- 望む実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼したり、コンテキストを追加したりして、反復が必要になる場合があります。Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
