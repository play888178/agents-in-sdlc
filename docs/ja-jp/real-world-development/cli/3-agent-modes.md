---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方針を決め、Autopilot で issue のフィルター機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

まず、プロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は、組織で既に使われている開発フローに沿って、適切な方法で必要なものを構築できるよう支援します。このレッスンから、エージェントを活用した一般的な開発プロセスに沿って作業します。issue を起点に機能を作り、コードを検証し、動作を確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> この機能のワークフローでは、同じ会話とブランチを使い続けます。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは中心となる概念に集中できるよう、簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot 会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

この機能の作業を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや発行元でゲームを絞り込める必要があります。バックログの issue には機能の説明がありますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定事項を整理し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、基本を守ることがさらに重要になります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記した issue から作業を始めます。
2. 構築するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くの場合、上記の流れを応用したものです。

この標準的な方法に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビュー プロセスを通るようにします。

## 会話モード

**会話モード**は、エージェントに与える自律性の程度を制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、続行前に開発者の入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルターの作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、続行する前に再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装を依頼する前に機能の方針や作業内容を定義できます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した issue の参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べ、最適な方法を検討して計画を作成します。

3. フィルター機能の動作について質問されたら、希望に沿って回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画をスクロールして確認します。
6. 決定事項と一致しない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクの完了を判断するまで作業を反復します。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンにある同様のラベルのオプションを選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画をレビューしてください。Copilot から依存関係不足やポート競合が報告された場合は、チェック完了と判断する前に環境の問題を解決します。

## 実装をレビューして検証する

生成されたコードも、他のコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** も **Autopilot** も出ていないことを確認します。
2. `/diff` を入力し、フィルターの実装とテストを確認します。
3. 結果を issue および計画時の決定事項と照らし合わせます。
4. コードを確認したら <kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードに問題がなさそうでも、実際に動くことを確認します。前回と同様にアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画時に合意したカテゴリ フィルター、発行元フィルター、および組み合わせを試します。
4. リセットと該当結果がない場合の動作が、issue と決定事項に一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを使い分けて機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot 会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された開発プラクティスに沿って生成される方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
