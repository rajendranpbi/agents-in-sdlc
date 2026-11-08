---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリの変更でも、複数ファイルの編集やテスト実行が必要になることがあります。Copilot Agent Mode は IDE 内で自律的にプロジェクトを調べ、関連ファイルを更新し、テストを実行して、見つかった問題を修正します。

> [!NOTE]
> agent mode と cloud agent は異なる体験を提供します。agent mode は IDE 内で短いフィードバック サイクルを実現します。cloud agent は Issue とプルリクエストを通じて非同期にチームの一員のように作業します。

この演習では、Copilot Agent Mode がプロジェクトを調べ、UI とデータ レイヤーを更新し、テストを作成する方法を確認します。生成された変更とテストをマージ前にレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと発行元による絞り込みを追加します。前の演習で発行元ヘルパーを作成しました。残りのデータ レイヤー、UI、テストを Copilot Agent Mode で仕上げます。

## Tailspin Toys の Web サイトを実行する

変更前に Web サイトを起動し、現在の機能を確認します。Tailspin Toys は開発者をテーマにしたボードゲームのクラウドファンディング サイトです。単一の Astro アプリがビルド時に静的 HTML を生成し、ページ frontmatter から Drizzle ORM を使ってローカル SQLite データベースを参照します。別のバックエンド API やクライアント UI フレームワークはありません。データ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作には Astro の小さな `<script>` と標準 DOM API を使います。

### Web サイトを起動する

1. Codespace に戻ります。前の演習後、`main` にいることを確認します。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) でターミナルを開きます。
3. 絞り込み機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 開発サーバーを起動します。

   ```bash
   npm run dev
   ```

Astro サーバーが起動すると、次のような案内が表示されます。

```bash
🚀 Tailspin Toys is ready!
   Astro server: http://localhost:4321
   Press Ctrl-C to stop.
```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルの `http://localhost:4321` を選択します。Codespace では `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。

### Web サイトを確認する

ホームページには、ボードゲームのタイトル、画像、説明が一覧表示されます。ゲームを選ぶと、タイトル、説明、発行元、カテゴリなどが掲載された詳細ページが開きます。

## Copilot でバックログを確認する

テンプレートから作成したプロジェクトには、Issue のバックログが用意されています。Copilot に優先順位を確認します。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace の Copilot Chat パネルを開き、**New Chat** で新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。

1. Chat ビューの agent picker で **Agent** を選びます。
2. 利用可能なモデルから **Claude Sonnet 4.5** を選びます。

> [!CAUTION]
> このワークショップは Claude Sonnet 4.5 を使って作成しましたが、特定のモデルを推奨するものではありません。モデルの出力は確率的であり、例と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できます。

3. 次のプロンプトを送信します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため **Continue** を選びます。
5. Issue の一覧と Copilot が提案した優先順位を確認します。

## 命令ファイルを確認する

コード生成を依頼する前に、Copilot にコンテキストを与える `.github/instructions/ui.instructions.md` を確認します。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. アーキテクチャ、コンポーネントの設計、テスト容易性、アクセシビリティ、および Astro、Tailwind CSS、Drizzle 用の命令へのリンクを確認します。

> [!TIP]
> 命令ファイルから別の命令やプロジェクト ファイルを参照できます。パスは命令ファイルからの相対パスです。命令を再利用し、大きなセットを小さく分け、参考例を示せます。

## 絞り込み機能を実装する

絞り込みには、データ レイヤー (`src/lib/`)、テスト、ゲーム一覧ページの UI を更新する必要があります。Copilot Agent Mode に依頼します。

1. 現在の会話を続けるか、**New Chat** を選びます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 命令ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. agent picker で **Agent** が選択されていることを確認します。
4. **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がファイルを調べ、データ ヘルパー、UI、テストを更新する様子を確認します。
7. テスト実行を求められたら **Continue** を選択します。作業中にテストが失敗することがあります。Copilot がコードとテストを反復して問題を解決するまで待ちます。
8. 生成されたコードを確認します。**Keep** と **Undo** は作業中に選ぶ必要はなく、完了後に変更を確認して選びます。
9. コードをレビューし、起動中のブラウザーで機能を試します。
10. 問題がなければ Copilot Chat で **Keep** を選びます。

## まとめと次のステップ

GitHub Copilot Agent Mode でデータ レイヤーと UI に機能を追加し、プロジェクトを調べて関連ファイルを更新し、生成された変更とテストをレビューしました。次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、PR を作成します。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えた場合に備え、Copilot にページ分割の実装を依頼できます。既存の命令を使い、ページ サイズをユーザーが選ぶか固定するかなどの要件を明確に伝え、必要に応じて反復してください。

## リソース

- [Copilot の ask、edit、agent mode の使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
