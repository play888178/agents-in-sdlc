---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** は、GitHub Copilot をクラウドで非同期に動作させます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を加えてプルリクエストを作成します。その間、別の作業を続けられます。

このワークショップでは、まず cloud agent が従うカスタム指示を追加し、GitHub issue を割り当てて実装を任せます。作業の進め方をカスタム エージェントで調整し、agents ダッシュボードからセッションを監視して方向付けます。最後に、作成されたプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加する |
| [2. GitHub Copilot cloud agent][ex2] | 非同期エージェント | issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 専門エージェント | カスタム エージェントを確認して使う |
| [4. エージェントの監視と管理][ex4] | 監視 | エージェント セッションを監視し、方向付ける |
| [5. 反復して改善する][ex5] | レビュー | PR をレビューし、Copilot の作業を反復して改善し、次のステップを選ぶ |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] アカウントまたは組織で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランをお持ちでない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できます。Copilot Business または Enterprise では、組織の管理者が有効にする必要があります。

> [!NOTE]
> この harness では MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI harness](../cli/) または [VS Code harness](../vscode/) をご覧ください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
