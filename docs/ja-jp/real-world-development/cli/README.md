---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys の変更を 2 件実装、検証、リリースします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、GitHub Copilot をエージェント型コーディング アシスタントとしてターミナルで利用できるようにします。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys で一連の作業を進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本操作を確認します。
2. 星評価の変更を加え、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルターの issue から始め、Plan モードで方針を決め、Autopilot モードで実装してから、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルターの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルターの動作を調べます。
7. quality assurance (QA) 用のカスタム エージェントを作成し、要件、テスト範囲、検証の証拠を確認します。
8. フィルターの変更全体をレビューし、Agent Merge を使ってフィルターの PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に便利なスラッシュ コマンドを確認します。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルターの 2 つの PR を作成します。フィルターと品質確認の作業では、1 つの会話とブランチを続けて使い、各ツールで得た内容を次の作業に生かします。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本操作を確認する |
| [2. 星評価を追加する: 手早く成果を出す][ex2] | 最初の変更 | 既存の評価と `null` のフォールバックを表示して、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で実装してから Interactive モードでレビューする |
| [4. カスタム指示で Copilot を導く][ex4] | コンテキスト | 指示を確認、更新して、フィルターの作業に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 再利用可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | CLI で MCP を構成し、フィルターの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門プロフィールを作成して選択し、最終検証の証拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューして PR を作成し、Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI の機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用できるカスタマイズ、追加リソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログの情報に基づくエージェントをデプロイして、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
