---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同様に、GitHub Copilot CLI にも操作用のスラッシュ コマンドが多数あります。コマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ほかの便利なコマンドも確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストをどう管理するかを確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の基本的なワークフローを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュ コマンドを使って自分で確認・圧縮することもできます。

1. Codespace に戻ります。Copilot CLI がまだ開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう分配されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合、大きな変化はないことがあります。

> [!NOTE]
> Copilot CLI はコンテキスト ウィンドウがいっぱいになると、自動的にコンテキストを圧縮します。タイミングを自分で選ぶ場合は `/compact` を使います。関係のない別のタスクに移り、新しい会話を始める場合は `/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意なことが異なり、開発者によって好みも違います。Copilot CLI では、利用できるモデルの一覧表示と選択ができます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、用途に合う方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの主要な部分は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分けて並行実行できます。
- `/worktree` は、別の作業用に独立した Git worktree を作成できます。
- `/delegate` は、非同期で作業してプルリクエストを開くことがある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生する場合があるため、これらのコマンドは任意です。試す場合は、新しく範囲を明確にしたタスクから始め、通常のワークフローで結果をレビューしてください。非同期のエージェント作業を詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュ コマンドを使って Copilot CLI を構成し、セッションを共有し、内部の動作を確認しました。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストをどう管理するかを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはまだ多くのスラッシュ コマンドがあります。次は、学んだ内容と今後の学習方法を[振り返り][next-lesson]、ワークショップを締めくくりましょう。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
