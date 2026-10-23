---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能を確認しました。次は、これらの機能を明確な品質保証 (quality assurance、QA) プロセスを持つカスタム エージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルターの issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を開く前に、要件、コード品質、自動チェック、テスト範囲、ブラウザー上の動作を一貫してレビューしたいと考えています。カスタム エージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、Markdown プロファイルで定義する、特定の作業に特化した Copilot です。プロファイルでは、エージェントの目的、指示、利用可能なツールを説明します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチーム標準を説明します。`quality-checks` スキルは繰り返し使えるチェックをまとめます。Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、要件を評価して結果を報告するためにそれらをどう使うかを Copilot に伝えます。他の機能を置き換えるものでも、別の会話を必要とするものでもありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。このプロファイルでは、QA が行うチェックと守るべき境界を定義します。

1. Codespace に戻り、フィルターの会話が Interactive モードであることを確認します。
2. 既定のエージェントに新しいカスタム エージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと制約がプロファイルに記載されていることを確認します。これにより、確認だけを依頼したのに、不完全または範囲が広すぎるエージェントが機能を変更することを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件を起点とし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を添えて報告し、実装コードを変更する前に確認を求め、変更をコミットしたり PR を開いたりしないことを確認します。
5. これらの役割や制約が抜けている場合は、続行する前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルを作成した後、同じフィルターの会話を再開して QA を選択し、issue と計画の決定事項を利用できるようにします。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されないことがあります。上記のコマンドで有効にできます。

2. 次のプロンプトで、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 完了後、生成されたレポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その作業をレビューしました。このレッスンでは、次のことを行いました。

- カスタム エージェントが指示、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な quality assurance プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルターの issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろいました。次は、これらを機能の PR にまとめ、Agent Merge を使います。[機能の PR を作成して Agent Merge を使う][next-lesson]に進んでください。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
