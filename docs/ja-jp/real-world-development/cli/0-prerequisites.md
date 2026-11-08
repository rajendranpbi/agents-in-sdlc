---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備をします。Tailspin Toys リポジトリの自分用コピーを作成して [codespace][codespaces] を起動します。次のレッスンでは、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の操作を行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトの準備ができたことを確認します。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][tailspin-template]から作成してください。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]に移動します。
2. **Use this template**、続いて **Create a new repository** を選択して、自分用のリポジトリを作成します。
3. GitHub または Microsoft が主催するイベントでワークショップを進めている場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot にアクセスできる organization に新しいリポジトリを作成します。
4. 作成したリポジトリのパス（`organization-or-user-name/repository-name`）を控えておきます。ワークショップの後半で使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップではこの Issue を使うため、自分で作成する必要はありません。

ワークショップ テンプレートの新しいコピーを使ってください。リポジトリには、指示、アプリケーション コード、テスト、`quality-checks` スキル、使用するバックログが含まれています。古いコピーを使用する場合は、必要なファイルが揃っているかファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザーから直接コードを作成、実行、デバッグできるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、フル機能のエディターを提供します。

1. 作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートによって、プロジェクトの依存関係、Playwright Chromium、ローカル データベースがインストールされます。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリのルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択し、Tailspin Toys サイトが表示されることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備ができました。このレッスンでは、Tailspin Toys プロジェクトの自分用コピーを作成し、Codespace を用意してプロジェクトが動作することを確認しました。

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
