---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。作業を反復しながら、変更を計画し、コードを作成し、動作をテストし、プルリクエストをレビューして、エージェントと作業を管理します。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys の新しい開発者として作業します。チームのバックログは GitHub issue として登録済みです。フィルターやページネーションなどの機能に加え、アクセシビリティやコーディング標準の改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。希望する開発方法に合った環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、便利なスラッシュ コマンドを活用します。

### [GitHub Copilot app][app]

デスクトップ アプリで複数のエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、その作業を案内し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントをエディターから離れずに活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
