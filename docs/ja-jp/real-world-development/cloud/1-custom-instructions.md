---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使う際にはコンテキストが重要です。作業の進め方に決まりがある場合や、Copilot に背景情報を伝える必要がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」作業するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI の harness とは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで影響を比べるのが難しいためです。この harness の後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- データ層には必ずユニット テストを追加します。
- UI はダーク モードにして、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこうした開発プラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体に適用する指示も、特定の種類のファイルや作業に適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される、リポジトリ全体に適用する指示ファイルです。技術スタック、構築するものの概要、ベスト プラクティスなど、ほとんどのチャットや CLI のリクエストに関係するプロジェクト情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の作業やファイルの種類に適用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などの作業に対するガイドラインを記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
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
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトにある [UI 更新用][ui-instructions]や [Astro 用][astro-instructions]などのファイルを確認してください。
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

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc コメントとファイル先頭のコメント ブロックで記述する、プロジェクト全体のルールを追加します。後の演習で issue を処理するとき、cloud agent はこの指示を参照します。

編集の前にブランチを作成します。cloud agent は issue の対象ブランチにある指示を読み取りますが、ここでは変更を整理してコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 指示の変更をコミット、プッシュ、マージする

cloud agent は issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Copilot に issue を割り当てると、`main` からブランチを作成します。したがって、cloud agent が指示を参照できるよう、変更を `main` に反映する必要があります。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で issue を割り当てるとき、cloud agent がこの指示を参照できるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合、ブランチ作成の手順を省略し、`main` に直接コミットします。ここでは実際のプロジェクトに近い手順を体験できるよう、ブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルが Copilot にコンテキストを伝える方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。この harness の後半で、cloud agent が作成するプルリクエストをレビューするときに、追加した標準を確認できます。

次は [Copilot cloud agent に最初の issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
