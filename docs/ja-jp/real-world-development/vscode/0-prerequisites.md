---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、Tailspin Toys リポジトリのコピーと作業用の [Codespace][codespaces] を準備し、エディターで GitHub Copilot Chat が使えることを確認します。

## ワークショップ用リポジトリをセットアップする

テンプレートからワークショップ用のリポジトリを作成します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template**、続けて **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 後で使うため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えます。

> [!NOTE]
> **バックログは準備済みです**
>
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログも自動的に作成されます。ワークショップではこの Issue を使うため、自分で作成する必要はありません。

## Codespace を作成する

演習では Codespace を使います。[GitHub Codespaces][codespaces] は、ブラウザーでコードを記述、実行、デバッグできるクラウド開発環境です。複数の言語、拡張機能、ツールをサポートする統合開発環境を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** を選択して Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

作成には数分かかります。その間に GitHub Copilot の機能を確認できます。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。現時点では、ブラウザーのタブを開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。ローカル環境を使う場合は、VS Code でクローンしたリポジトリを開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

## GitHub Copilot Chat と agent mode を使う

IDE で GitHub Copilot Chat の agent mode を使うには、GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace には通常、すでにインストールされています。

> [!TIP]
> 拡張機能がない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]するか、VS Code の Extensions ビューで **GitHub Copilot Chat** を検索して **Install** を選択します。

インストール後、GitHub アカウントでの認証が必要になる場合があります。

1. Codespace に戻ります。
2. エディター右側に Copilot Chat が表示されていない場合は、Codespace 上部の **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに `Hello world` などのメッセージを入力して <kbd>Enter</kbd> を押します。
4. サインインを求められた場合は、画面の指示に従って認証します。
5. 認証後、Copilot Chat ウィンドウが表示されていることを確認します。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

## まとめ

ワークショップ用リポジトリのコピーを作成し、コードを書くための Codespace の作成を開始しました。

## 次のステップ

次は [演習 1: カスタム命令][next-lesson]に進み、Copilot にプロジェクトの規約を伝えます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces]: https://github.com/features/codespaces
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
