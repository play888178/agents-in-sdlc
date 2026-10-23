---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys で一連の作業を進めました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、フィルターの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの issue を起点に Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルターを確認しました。
- quality assurance (QA) 用のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルターの変更全体をレビューし、フィルターの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

ワークショップには、更新した `main` からそれぞれ別のブランチで作成する、2 つの PR の節目があります。

1. **星評価:** 既存の `starRating` と、未評価であることを示す状態をゲームカードに表示します。
2. **フィルターと品質確認のワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズして、QA プロファイルと関連テストを追加します。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップの手順を簡潔にするため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを検証しました。`quality-checks` スキルは、ユニット テスト、lint、型チェックを実行して、新しい形式で結果を報告しました。QA は、要件とテスト範囲のレビューにこれらの結果をまとめて、PR の前に確認しました。

追加するテストは、実際の不足を補うものであるべきです。QA の実行で新しいテストが不要という結果になる場合もあります。マージを許可する前に、コードと証拠をレビューし、変更後は影響を受ける証拠を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールは、生成される結果に影響します。このワークショップでは、指示の更新、スキルのカスタマイズ、QA プロファイルの作成、MCP サーバーの構成を行いました。こうしたカスタマイズは、会話をまたいで再利用し、チームの必要に応じて調整できます。指示は標準を設定し、スキルは繰り返し行う作業を説明し、カスタム エージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前に方針を考えるには **Plan**、範囲の明確な変更を対話しながら進めるには **Interactive**、適切に範囲を定めた作業を任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローを確認しました。Copilot CLI には、ほかにも次のような便利な機能があります。

- `/review` でコード レビュー エージェントに変更の分析を依頼します。
- `/rubber-duck` で問題について話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別の作業を独立させます。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。本番コード、趣味のコード、あるいはいつか作ろうと思っていた小さなアプリに活用してみてください。学んだことをチームで共有し、チームの経験からも学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境も試すには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
