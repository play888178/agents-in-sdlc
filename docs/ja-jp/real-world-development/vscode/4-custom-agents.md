---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定の作業や領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、指示、ベスト プラクティス、コーディング パターン、専門知識を定義すると、特定の作業をより効果的に行えるよう Copilot を導けます。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML (YAML Ain't Markup Language) フロントマターと、そのエージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキルや他のエージェントを調整します。このラボでは、アクセシビリティ用カスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフロー スキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせてテストし、反復して改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚の能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマではテキストと背景色のコントラストが十分でなく、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot の作業にどう役立つかを把握します。

codespace に戻り、ターミナルで `main` から新しいブランチを作成して、アクセシビリティ作業を始めます。フィルターの PR は演習 3 のブランチに分けておきます。

1. 次のコマンドを実行します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

2. `.github/agents/accessibility.md` を開きます。
3. `name` と `description` フィールドを含む YAML (YAML Ain't Markup Language) フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

4. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - ハイパーテキスト マークアップ言語 (HTML)、Cascading Style Sheets (CSS)、JavaScript のコード例
   - よくある問題や間違い

## Copilot Chat でカスタム エージェントを使う

VS Code では、`.github/agents` に定義されたすべてのカスタム エージェントが Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選択すると、そのエージェントの指示とツールを使うチャット セッションにできます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な設定は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。現在のエージェント (既定では **default**) が表示されています。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案し、プロジェクト ファイルに編集を適用します。編集内容はチャットにファイル パスと差分とともに表示されます。
6. 編集が適用されるたびに、エージェントが変更したファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot は Astro コンポーネント、CSS、関連テストなどの既存ファイルを編集し、リポジトリに実際の変更を加えています。

次の演習では、作業中のセッションを確認し、方向付けます。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントは特定の作業や領域に特化した AI アシスタントです。チームの専門知識や標準を再利用できる形にまとめ、特定の作業をより効果的に進めるよう Copilot を導けます。

この演習では、次のことを確認しました。

- カスタム エージェントの定義方法
- Copilot Chat のエージェント モードでカスタム エージェントを使う方法

次は、[エージェントの作業を監視して方向付けます][next-lesson]。作業中の変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [統合開発環境 (IDE) でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
