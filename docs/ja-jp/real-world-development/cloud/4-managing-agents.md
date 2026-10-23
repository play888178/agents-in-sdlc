---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、Copilot cloud agent にユーザー エクスペリエンスの改善や機能追加に関する 3 つの作業を依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

GitHub の [agents ページ][agents-page]など、cloud agent に割り当てた作業を管理する方法はいくつかあります。この mission control では、開いているプルリクエスト (PR) があるエージェントの作業を確認できます。実行中のセッションを調べ、必要に応じて追加の指示を出すこともできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent の作業を監視します。
- 実行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加することにしました。サイトのスタイルと切り替え機能を更新しているため、この機能も含めるのが合理的です。エージェントの作業を方向付けて、ハイ コントラストに加えてライト モードも追加するよう依頼します。

## Copilot cloud agent の作業を確認する

Copilot cloud agent に割り当てた作業の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインに表示される作業の一覧を確認します。割り当てた作業として、次の項目が表示されるはずです。
   - コードのドキュメントを更新する作業
   - ゲームを変更する API を生成する作業
   - Web サイトにハイ コントラスト モードを追加する作業
3. 実行中の作業を選択し、Copilot が行った処理を確認します。次のような作業が含まれる場合があります。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーの設定
   - 割り当てられた作業を完了するための各種処理

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent に追加の指示を出す

実行中の作業を確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう依頼します。

1. ハイ コントラスト モードの作業に関するセッションを選択します。Copilot が付けたタイトルや作業状況によって、表示される名称は異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。作業開始後は、以下の例のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![cloud agent の作業画面で、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に含めたことを確認します。

## Copilot の作業を続ける

Copilot は更新された作業に取り組みます。メッセージを送信した時点で進行中の手順が完了した後、新しい依頼を作業に取り入れます。

完了までには数分かかります。その間に、これまでに学んだ内容を振り返りましょう。

## まとめと次のステップ

Copilot cloud agent の作業を監視して指示する中心的な場所である Copilot agents ページを確認しました。mission control では、実行中と完了済みの作業を追跡し、処理内容をレビューして、進行中の作業に追加の指示を出せます。

このレッスンでは、次のことを行いました。

- mission control と agents ページを使って cloud agent の作業を監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を作成している間に、Copilot が作成した[プルリクエストを反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
