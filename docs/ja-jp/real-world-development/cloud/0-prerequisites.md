---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、[Codespace][codespaces] を起動します。Codespace では、命令ファイルを編集し、cloud agent が作成した変更をレビューできます。

## ワークショップ用リポジトリをセットアップする

ワークショップで使うコードを用意するため、[テンプレート][template-repository]からリポジトリを作成します。作成したリポジトリには必要なファイルが含まれます。以降の演習ではこのリポジトリを使います。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリ ページで **Use this template** ボタンを選択し、**Create a new repository** を選択して自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス（**organization-or-user-name/repository-name**）を控えておきます。ワークショップの後半で使用します。

## Codespace を作成する

続いて、演習で使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーでコードを記述、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする統合開発環境 (IDE) を利用できます。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

    ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい Codespace を作成します。

    ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、必要なサービスを手動でインストールするよりも早く完了します。作成中に GitHub Copilot の機能を確認しておきましょう。次の演習で詳しく扱います。

> [!CAUTION]
> 今後の演習で Codespace に戻ります。現時点では、ブラウザーのタブを閉じずに開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件がインストールされた環境です。ローカル環境を使う場合は、VS Code でクローンしたリポジトリを開き、プロンプトが表示されたら **Reopen in Container** を選択します。Codespace と同じ dev container が構築されます。

## まとめ

ワークショップ用リポジトリのコピーを作成し、Copilot cloud agent と作業するための Codespace の作成を開始しました。

## 次のステップ

次は、cloud agent が従うカスタム命令を追加します。[演習 1: カスタム命令][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
