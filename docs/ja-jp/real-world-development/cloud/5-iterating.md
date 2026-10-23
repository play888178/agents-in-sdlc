---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能追加を目的として、GitHub Copilot にいくつかの作業を依頼しました。コードへのドキュメント追加、デザイン チームが反復できる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまで何度も説明したとおり、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進めます。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ監視なしで作業するため、安全性を確保するセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みアクセスは作業用ブランチに限定されます。
- cloud agent は GitHub Actions 内で動作し、作業用に一時的な独立環境を作成します。
- GitHub Actions ワークフローは、実行前に人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されており](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)、Model Context Protocol (MCP) サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。これはコードにドキュメントを追加する PR です。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールにブロックされたという警告が表示される場合があります。Copilot は既定で外部リソース (外部 MCP サーバーへの呼び出しなど) へのアクセスが制限されているため、これは想定内です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation**、またはそれに近いタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** フラグが付いています。その場合は Copilot の作業が完了するまで待ちます。数分かかることがあるため、休憩したり、ここまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc ドキュメント コメントなど、更新されたコードを確認します。変更内容は状況によって異なります。

    差分を確認しながら、TSDoc ドキュメント コメント、TypeScript 規約、ファイル先頭のコメントを探します。これらは演習 2 の最初に確認したカスタム指示ファイルに由来します。

6. 更新内容をレビューし問題がなければ、**Conversation** タブに戻り、下へスクロールします。
7. 一部のワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローが実行待ちになっていることを確認します。順調に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、プルリクエストやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを認識し、対応する新しいセッションを開始します。結果は決定論的ではないため、依頼する内容を厳密に指定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメントを追加する。
- TypeScript ファイルと Astro ファイルに TSDoc ドキュメント コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. ユーザーにメンションするのと同じ方法で **@copilot** を付け、生成されたドキュメントの変更をコメントで依頼します。上記の例や、コードベースのドキュメントに関する別の提案を使えます。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択して PR に戻ることもできます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

納得できるまで Copilot との反復を続けてください。準備ができたら、ドラフト PR をレビュー準備完了に変更し、`main` ブランチにマージできます。

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、またはそれに近いタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容をレビューし問題がなければ、**Conversation** タブに戻り、下へスクロールします。
6. 一部のワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄で、ワークフローが実行待ちになっていることを確認します。順調に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。先ほどと同じように、Copilot cloud agent に変更を依頼して反復できます。たとえば、表示する関連ゲームの数を変更したり、コメント ヘッダーと TSDoc ドキュメント コメントを追加したりできます (この issue は、カスタム指示を更新する**前**に割り当てたことを思い出してください)。前と同じく、**Conversation** タブにコメントを追加します。Copilot がコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタム エージェントで実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で mission control から追加依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、またはそれに近いタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** フラグが付いています。作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して生成されたコードをレビューします。
5. 次の点に注目して実装を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイ コントラスト モードとライト モードに関する CSS またはスタイルの変更
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)
   - モード切り替えを処理する JavaScript/TypeScript コード

6. 更新内容をレビューし問題がなければ、**Conversation** タブに戻り、下へスクロールします。
7. 一部のワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄で、ワークフローが実行待ちになっていることを確認します。順調に進めば、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。codespace に移動し、ターミナルを開いて、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ用カスタム エージェントが、アクセシビリティのベスト プラクティスに沿った機能の実装を Copilot に促したことを確認します。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** に変更を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent に実際のバックログ項目を任せ、その間に別の作業を進める方法を身につけましょう。リポジトリに issue をいくつか登録し、Copilot に割り当てます。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリにあるカスタム指示ファイルを確認し**、cloud agent の出力に与える影響を把握しました。
- **issue を Copilot cloud agent に割り当て**、環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタム エージェントを作成して使用し**、ハイ コントラストとライト モードの切り替えを追加しました。
- **Copilot agents ページを mission control として使用し**、アクセシビリティ セッションを監視して途中で方向付けました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

Cloud agent harness を完了しました。さらに学びたい場合は、次の harness で今回の内容を別の方法で試せます。

- 🖥️ **[VS Code harness](../../vscode/)** — 統合開発環境 (IDE) で Copilot Chat エージェント モードと MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan と Autopilot、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` ブリッジを使い、ターミナルから変更を 2 件レビューしてリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野を担当するカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
