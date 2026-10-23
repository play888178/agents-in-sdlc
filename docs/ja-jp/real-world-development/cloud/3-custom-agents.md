---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定の作業や領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、指示、ベスト プラクティス、コーディング パターン、専門知識を定義すると、特定の作業をより効果的に行えるよう Copilot を導けます。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを保つテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには少なくとも `name` と `description` を含む YAML フロントマターと、そのエージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。違いを簡潔に説明すると、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一環としてスキルや他のエージェントを調整します。このラボでは、アクセシビリティ用カスタム エージェントがガイドラインに沿ってサイトをレビューし、更新します。作業中に、プルリクエストのワークフロー スキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法に唯一の正解はありません。AI を使う他の作業と同じように、環境やシナリオに合わせてテストし、反復して改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタム エージェントについて次のことを確認します。

- カスタム エージェントの定義方法
- エージェントへの作業の割り当て方

## シナリオ

Tailspin Toys は、視覚の能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを利用できるように取り組んでいます。最近、現在のダーク テーマではテキストと背景色のコントラストが十分でなく、読みにくいというフィードバックがありました。このアクセシビリティの問題に対処するため、デザイン チームは利用者が切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使ってこの機能をすばやく実装します。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot の作業にどう役立つかを把握します。

codespace に戻り、次の手順でアクセシビリティ用カスタム エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコードを生成する際の主な責務
   - アクセシビリティのベスト プラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## issue を作成して割り当てる

mission control は、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent への作業の割り当て、進捗の監視、実行中セッションへの追加指示を行えます。ハイ コントラスト モードを作成する作業を Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタム エージェントと Accessibility が選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot のカスタム エージェントについて学びました。カスタム エージェントを使うと、特定の作業や領域に特化した AI アシスタントを作成し、チームの専門知識や標準を再利用できる形で Copilot に伝えられます。

この演習では、次のことを確認しました。

- カスタム エージェントの定義方法
- エージェントへの作業の割り当て方

Copilot がハイ コントラスト モードを実装している間に、[mission control からエージェント セッションを監視して方向付ける][next-lesson]方法を確認しましょう。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
