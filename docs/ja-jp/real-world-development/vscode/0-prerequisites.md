---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーと作業用の [codespace][codespaces] を作成し、エディターで GitHub Copilot Chat が動作することを確認します。

## ラボ用リポジトリを設定する

演習で使うコードのコピーを作成するため、[テンプレート][template-repository]からリポジトリを作成します。作成したリポジトリにはラボに必要なファイルが含まれており、演習を通して使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. ラボ リポジトリのページで **Use this template** を選択し、**Create a new repository** を選択します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。後の演習で使用します。

> [!NOTE]
> **バックログの準備**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこれらの issue を使うため、自分で登録する必要はありません。

## codespace を作成する

続いて、codespace を使って演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザー上でコードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、機能を備えた IDE を利用できます。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい codespace を作成する](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、サービスを手作業でインストールするよりもずっと速く準備できます。その間に、次に扱う GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 今後の演習で codespace に戻ります。それまでは、ブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace、またはローカルの [dev container][dev-containers] で実行できます。どちらもスムーズに作業できるよう、必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択してください。codespace と同じ dev container が VS Code によって構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェント モードを使う

VS Code の GitHub Copilot Chat エージェント モードを使うには、IDE に GitHub Copilot Chat 拡張機能がインストールされている必要があります。GitHub Codespaces を使っている場合は、すでにインストールされています。

> [!TIP]
> GitHub Copilot Chat 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または、Visual Studio Code の **Extensions** ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

拡張機能をインストールした後、利用を開始するために GitHub アカウントで認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace 上部の **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに「Hello world」のようなメッセージを入力し、<kbd>Enter</kbd> を押します。これで Copilot Chat が起動します。
4. まだ認証されていない場合は、GitHub アカウントへのサインインが求められます。画面の指示に従って認証します。

    ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されることを確認します。

## まとめ

ラボ用リポジトリのコピーを作成し、コードを書き始めるときに使う codespace の作成を開始しました。

## 次のステップ

Copilot を使い始めましょう。[演習 1 - カスタム指示][next-lesson]に進み、プロジェクトの規約を Copilot に伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
