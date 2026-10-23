---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使う際にはコンテキストが重要です。作業の進め方に決まりがある場合や、Copilot に背景情報を伝える必要がある場合は、その情報を参照できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。指示ファイルを使ってコード生成のガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- *現在の*指示を適用した状態で、フィルター機能の最初のデータ スライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度実行し、新しい標準が生成コードに反映される様子を確認します。
- 次の演習で Copilot が更新後の指針を使えるよう、指示の更新とフィルターの基礎部分を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが設定した標準と異なる場合があります。Copilot の動作は確率的です。この演習の目的は、出力を一字一句一致させることではなく、指示の更新による傾向の変化を確認することです。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には必ずユニット テストを追加します。
- ユーザー インターフェイス (UI) はダーク モードにして、現代的な見た目にします。
- コードには TSDoc ドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルや作業に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される指示ファイルです。技術スタック、構築するものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイルの種類に適用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などの作業に対するガイドラインを記述できます。

> [!NOTE]
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルのファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[指示ファイルの対応状況][custom-instructions-support]をご覧ください。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルを作成する方法のすべてをこのワークショップで扱うことはできませんが、サンプル プロジェクトには代表的な方法が含まれています。基本的な考え方は次のとおりです。

- `copilot-instructions.md` には、構築するもの、プロジェクトの構成、全体的なコーディング標準など、プロジェクト レベルの指針を記述します。
- `*.instructions.md` には、ユニット テスト、Astro コンポーネント、データ層など、ファイルの種類や特定の作業に関する指針を記述します。
- 自然な言葉で明確に書き、コードの良い例と避ける例を示します。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同様、試しながらプロジェクトに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトにある[ユーザー インターフェイス (UI) 更新用][ui-instructions]や [Astro 用][astro-instructions]などのファイルを確認してください。
>
> Copilot を使って指示ファイルを生成することもできます。操作は利用する環境によって異なります (VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init` など)。各環境のレッスンで、該当する方法を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、さまざまな作業用の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指針も確認してください。これらは Copilot を使う際に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリのルートを基準に、指示が適用されるファイルを決める glob です。この例では `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトでユニット テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` など他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用できる小さなファイルに分割し、コード生成時に参考例を示せます (パスはリポジトリのルートではなく、指示ファイルを基準にします)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントの要件はまだありません。次の手順で、TSDoc ドキュメント コメントとファイル先頭コメントのルールを追加します。

## 変更用のブランチを作成する

ここでは、ベスト プラクティスに沿って変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な設定は各手順で説明します。

カスタム指示の効果を確認するため、まずは現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけが渡るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの保存場所を Copilot に示します。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント ピッカーを示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルとテストなどのコードを更新します。
6. 提案されたヘルパーが `db` クライアントを最初の引数として受け取り、型付きの発行元配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc ドキュメント コメントとファイル レベルのコメント ヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加することがあります。その場合も問題ありません。この演習の目的は、指示の更新後に一貫性がどう変化するかを確認することです。

## リポジトリ全体に適用する標準を追加する

前に説明したとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を伝えます。コーディング標準を記載し、コード提案の改善に役立てます。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準を確認し、コード内ドキュメントのルールがないことを確認します。
3. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ全体の指針を記述する `.github/copilot-instructions.md` と、言語、ファイルの種類、作業を対象とする `*.instructions.md` のどちらにも作成できます。今回のようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話のコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot が新しいプロジェクト標準に従い、次の演習で使う実際のコードに適用するよう指示できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルも他の資産と同様、ソース管理で管理します。変更をコミットして、ブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤作成に Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (テストなど) が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. リポジトリにブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準に関する issue に関連付けたプルリクエストを作成します。手動でもできますが、GitHub ツールに接続済みの Copilot に依頼します。該当する issue を探し、PR を作成して issue をクローズし、マージするよう促します。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのはこの演習が初めてです。プロジェクト テンプレートには GitHub ツールがあらかじめ接続されています。Copilot が最初にツールを使う際、VS Code で GitHub へのサインインを求められる場合があります。表示される手順に従って許可してください。後の演習では、この接続に使われる Model Context Protocol について学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と発行元ヘルパーの基盤作成を説明する PR を現在のブランチから作成し、関連する issue を探すよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. Copilot が GitHub 操作を行う許可を求めたら、コマンドを確認し、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェル コマンドの実行を許可するよう求めたら、コマンドを確認して適切な場合は許可します。

GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを受け取る方法を確認し、VS Code の Copilot Chat でフィルター機能のデータ層を準備しました。具体的には、次のことを行いました。

- リポジトリの指示とパスを対象とする指示ファイルが、Copilot にどう適用されるかを確認しました。
- 現在の指示を使ってコード生成プロンプトを送信し、発行元ヘルパーの出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、生成されたコードに新しい標準が反映される様子を確認しました。
- 次の演習の基盤となる指示の更新とヘルパーをコミットし、`main` に反映しました。

次は、エージェント モードで指示を活用し、コードベース全体にまたがる新機能を Copilot に追加してもらいます。[エージェント モード][next-lesson]に進んでください。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
