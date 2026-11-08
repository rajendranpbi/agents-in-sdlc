---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーで絞り込み機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードの作成以外にも、データや外部サービスとの連携、Copilot が使える自動化機能の追加が必要です。Model Context Protocol (MCP) サーバーを使うと、Copilot CLI に組み込まれていないツールやサービスも利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認します。
- まだ利用できない場合は Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

ユニットテストやエンドツーエンド テストは重要ですが、ユーザー インターフェイス (UI) の更新を検証するには、実際に操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにし、変更の自動検証を支援させます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントは最新の情報にアクセスし、代理で操作できます。

MCP サーバーは、AI エージェントと外部ツールやサービスの間をつなぎます。各サーバーは、エージェントが利用できるツールやリソースを提供します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub はエコシステム内での発見や貢献を促すため、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。使用前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot が Web サイトをユーザーのように操作できるよう追加します。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに構成されている場合は、セットアップを飛ばして次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd>、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. Copilot に Playwright MCP サーバーを使って検証するよう、次のプロンプトを送信します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は、どの MCP サーバーを使うか Copilot が自動的に判断するため、明示する必要はありません。ここでは使うツールを指定し、結果の一貫性を高めます。

2. Copilot がブラウザーで機能を検証する手順を確認します。
3. レポートを読み、すべてが想定どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使って、実際のブラウザーで機能を確認しました。このレッスンでは、次の操作を行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を確認しました。
- まだ利用できない場合に Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は、スキルとブラウザー ツールを組み合わせる[QA カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
