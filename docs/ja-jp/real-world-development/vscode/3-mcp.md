---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

agent mode で絞り込み機能を作成しました。PR を開く前に、ブラウザーで動作を確認します。Playwright MCP サーバーを接続し、Copilot に実際のブラウザーを操作させてから、ブランチを公開して PR を作成します。

この演習では、Model Context Protocol (MCP) と MCP サーバーの役割を学び、Playwright MCP を追加して絞り込みを検証し、作業ブランチを公開して PR を作成します。

## Model Context Protocol (MCP) とは

MCP は、Copilot などの AI エージェントが外部のツールやサービスと通信するための標準です。MCP サーバーはエージェントとの間をつなぎ、最新情報へのアクセスやツールの実行を可能にします。

![agent mode がコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと連携する仕組みを示す図](../../../_images/ex1-mcp-diagram.png)

- **[GitHub MCP Server][github-mcp-server]** は、GitHub リポジトリ、Issue、プルリクエストを管理する API を提供します。
- **[Playwright MCP Server][playwright-mcp-server]** は、Web ページの移動、フォーム入力、ボタン選択などのブラウザー自動化を提供します。

ほかにも多くのサーバーがあり、GitHub は発見や貢献を促す[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同様に扱います。使用前にソース コードと発行元を確認し、セキュリティへの影響を検討してください。信頼できるサーバーだけを使い、機密リソースや操作へのアクセス許可に注意します。

## MCP の構成を確認する

`.vscode/mcp.json` は VS Code ワークスペースで使える MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. 次の `github` サーバーがすでにあることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

テンプレートにはリモート GitHub MCP サーバーが含まれています。ローカルへのインストールは不要で、VS Code が HTTP で接続します。Copilot が初めてツールを使うときに GitHub 認証を行います。

## Playwright MCP サーバーを追加する

ブラウザーを操作する Playwright MCP サーバーを追加します。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

`github` はリモート サーバーですが、`playwright` はローカル サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。Codespace には表示用デスクトップがないため、`--headless` でブラウザーを画面表示なしで実行します。

> [!NOTE]
> Tailspin Toys はエンドツーエンド テストに Playwright を使うため、ブラウザーは通常インストール済みです。見つからない場合は Copilot に `npx playwright install chromium` を実行させます。

## サーバーを起動して信頼する

VS Code は必要になったときに MCP サーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に表示される **Start** を選び、構成を確認します。
2. 信頼を求められたら、構成を確認してサーバーを信頼します。

手動で起動する必要はありません。次の手順でツールが必要になった際に、VS Code が起動して信頼を確認します。

## 絞り込み機能をテストする

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化要素を調べるため、機能確認と簡単なアクセシビリティ確認を同時に行えます。

アプリが起動している状態で、Copilot に次のプロンプトを送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot はブラウザーを起動して手順を実行し、結果を報告します。Issue の受け入れ条件に照らして確認し、問題があれば質問するか、PR を開く前にコードの修正を依頼します。

> [!NOTE]
> テストには `http://localhost:4321` でアプリが実行中である必要があります。開発サーバーを停止した場合は、プロンプト送信前に再起動します。初回はブラウザーのダウンロードが必要な場合があります。見つからないと報告されたら `npx playwright install chromium` を実行します。

## ブランチを公開して PR を作成する

機能の動作を確認したので、チームのレビュー用に `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルで Copilot の変更を確認します。
2. **+** を選択して変更をステージします。
3. **Sparkle** ボタンでコミット メッセージを生成します。
4. **Publish** を選択してブランチをリポジトリにプッシュします。

![Source Control に Copilot の変更が表示された画面](../../../_images/ex3-source-control-changes.png)

続いて、GitHub ツールを使う Copilot に PR を作成させます。

1. Copilot Chat で **New Chat** を選択し、agent picker で **Agent** を選びます。
2. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

3. 必要に応じて **Continue** を選び、情報取得や操作を許可します。GitHub ツールを初めて使うときは認証が必要になる場合があります。
4. Copilot が Issue を探して PR を作成したら、リンクから内容を確認します。まだマージしないでください。

## まとめと次のステップ

MCP サーバーが Copilot にツールを提供する仕組みを確認し、Playwright でブラウザーから機能をテストして、ブランチを公開し PR を作成しました。次は、アクセシビリティ確認などの作業を効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
