---
title: "レッスン 1 - GitHub Copilot CLI のインストール"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みのフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースの探索、コード生成、コマンドの実行、外部ツールとの連携をコマンド ラインから行えます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、Copilot と短いやりとりをします。
- 組み込み GitHub Model Context Protocol (MCP) server を使ってフィルター機能の Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに対応するため AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を提供します。このレッスンではインストールと認証を済ませ、以降のワークショップで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認してから、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。認証を求められずに Copilot CLI が起動した場合は、そのまま進めてください。

## 使い方を確認する

通常のシェル プロンプトに入力したコマンドは Codespace で実行されます。Copilot CLI の起動後は、自然言語でエージェントに指示し、スラッシュ コマンドで会話を制御します。

1. `/model` と入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` と入力して、インストール済みバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が正しく動作していることを確認するため、次の質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べていることを確認します。
5. `/mcp list` と入力し、組み込み GitHub MCP server が利用できることを確認します。
6. Copilot にフィルター機能の Issue を検索させます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された uniform resource locator (URL) を開き、Issue を読みます。最初の変更を行った後にこの Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的に worktree は作成されません。各変更の前に機能ブランチを作成します。

## ワークショップ用ショートカットを使う

Copilot CLI は通常、許可済みの範囲外でツールを使う前に確認します。このワークショップでは、Codespace 内の承認プロンプトを省略する、ユーザーが選択したショートカットとして `--yolo` を使います。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL に対する自動許可（`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`）を有効にします。Codespace や仮想マシン (VM) などの隔離された環境でのみ使い、日常の開発用の既定値としてエイリアスにしないでください。詳細は[ツールの使用許可と拒否][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` によって、後のレッスンで Issue やプルリクエストに使う読み取り／書き込み用 GitHub MCP ツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして会話できることを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、GitHub Copilot CLI を npm でインストールし、GitHub アカウントで認証しました。また、ワークショップのリポジトリを信頼して短いやりとりを行い、組み込み GitHub MCP server でフィルター機能の Issue を見つけました。

次は、[最初の小さな変更][next-lesson]として、Copilot CLI を使ってゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
