---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、バックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行し、完了すると人がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を委任し、開発者がより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次の操作を行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた Issue の重要性を確認します。
- Copilot に Issue を割り当てます。

## シナリオ

Tailspin Toys では、エクスポートされた関数に TSDoc コメントがありません。まず、アプリケーションのエクスポート関数にコメントを追加します。また、デザインチームはゲーム詳細ページに同じカテゴリの関連ゲームを表示し、受け入れテストで UX を試したいと考えています。優先度の高い作業を進めながら、これらのタスクを cloud agent に非同期で任せます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当て][assign-issue]ると、進行状況を追跡するドラフト PR を作成し、環境を準備してタスクに取り組みます。実行中または完了後にセッションを確認でき、レビューの準備ができると PR で通知されます。

## 範囲を適切に定めた命令の重要性

GitHub Copilot を使っても、考える作業を省略できるわけではありません。単純に見える操作にも複雑さがあるため、[cloud agent に依頼するタスクの範囲を適切に定めてください][cloud-agent-best-practices]。AI ペア プログラマーとして段階的に作業し、学び、試し、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習ではカスタム命令とその働きを確認しました。まだであれば、cloud agent に作業を割り当てる前に、リポジトリに含まれる命令ファイルを読み取り専用で確認します。

- `.github/copilot-instructions.md` の **Code standards** を読み、TypeScript の規約、TSDoc コメント、ファイル ヘッダーの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` frontmatter が `**/*.test.ts` に命令を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当て、これらの命令が PR に反映されることを確認します。

## cloud agent の開発環境をセットアップする

cloud agent は GitHub Actions を使って作業環境を準備します。`.github/workflows/copilot-setup-steps.yml` に専用のセットアップ ワークフローを置くと、必要な開発ツールや依存関係をインストールできます。このラボには、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー、単一 Astro アプリ用に移行および seed された SQLite データベースを用意するワークフローが含まれています。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

このワークフローには、`copilot-setup-steps` という 1 つのジョブがあります。Copilot が PR の作業を始める前に GitHub Actions で実行されます。`workflow_dispatch` を使うと、Copilot を待たずにリポジトリの **Actions** タブからワークフローを手動実行できます。

## ドキュメントを追加する Issue を作成する

ドキュメント不足は後回しになりやすく、保守や新しい開発者の参加を難しくします。Copilot cloud agent にドキュメント追加を依頼します。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues**、**New issue**、**Blank issue** の順に選択します。
3. **Title** に `Code lacks documentation` と入力します。
4. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

5. **Create** を選択して Issue を作成し、右側の **Assign to Copilot** を選択してダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

6. **Assign** を選択します。
7. **Pull Requests** タブを開き、`[WIP]: Code lacks documentation` のようなタイトルの PR を探します。表示されない場合は少し待ってからページを更新します。
8. Copilot のタスク一覧が表示されたら、実行する作業を確認します。環境の作成や解析に数分かかることがあります。
9. PR のタイムラインで Copilot が作業を開始したことを確認し、**View session** を選択します。更新表示が見つからない場合はページを更新します。

   ![Copilot セッションを表示する画面](../../../_images/ex4-view-session.png)

セッションでは、Copilot がコードを調べて計画を立て、実装する様子を確認できます。処理中は、次の作業に移る間に別のタスクを割り当てます。

## ゲーム詳細ページに関連ゲームを追加する Issue を作成する

cloud agent が非同期で作業している間に、デザインチームの関連ゲーム機能も依頼します。

1. github.com のリポジトリに戻り、**Issues**、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Show related games on the game details page` と入力します。
3. **Description** に次の要件を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

4. **Create** を選択して Issue を作成し、右側で **Assign to Copilot** を選択します。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

5. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示されると、Copilot が作業を開始したことを示します。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

cloud agent は非同期に作業するため、完了まで常に監視する必要はありません。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] に Issue を割り当て、技術的負債の解消、新機能の作成、フレームワーク移行などを非同期で進められることを確認しました。この演習では、次の操作を行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を確認しました。
- Copilot に Issue を割り当てました。

cloud agent が作業している間に、次はカスタム エージェントを作成して使います。[cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、カスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
