---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際にはコンテキストが重要です。作業に特定の進め方がある場合は、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も伝えられます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、自分のコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスを対象とする指示ファイルが、エージェントにどのように渡されるかを確認します。
- コーディング標準を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発方法に関するガイドラインと要件があります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、引数、戻り値を説明する TSDoc/JSDoc を追加します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードの変更時にはコメントも最新の状態に保ちます。
- 既存のフォーマットと lint の指針を維持します。

指示ファイルを使い、Copilot がこれらの開発プラクティスに沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件などのコンテキストを Copilot に伝え、より適切な提案やコード スニペットを生成できるようにします。好みの規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルや作業に適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される指示ファイルです。ほとんどのリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイルの種類、作業に関するガイドラインを提供します。

> [!NOTE]
> その他の指示形式と対応状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには指示ファイルがすでに用意されています。変更による影響を確認する前に、既存の内容を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この指示は、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成時に参考例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、改善の余地があります。コアの `copilot-instructions.md` ファイルを変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 次の項目をセクションの最後に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新後の指針を使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルター機能のコンテキストを保ったまま会話を終了して再開し、新しい指針を読み込ませます。

1. フィルターの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチに切り替わっていることを確認し、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたら、フィルターの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトで、更新した指針を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新たに追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルのコンテキストを利用する方法を確認し、新しい標準を機能に適用しました。次は、再利用可能な quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行する方法を確認します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
