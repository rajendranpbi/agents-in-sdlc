---
title: "レッスン 2 - 星評価を追加する: まずは小さな改善"
description: "GitHub Copilot app で最初のエージェント セッションを開始し、ゲーム カードを変更してブラウザーで確認し、最初のプルリクエストをマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースの確認とクイックチャットができました。次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。ゲームのデータには星評価がありますが、ホームページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- エージェント セッションを開始し、セッションの構成を確認します。
- エージェントに小さく範囲を絞った変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲーム カードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の練習として、各カードに既存の評価を表示します。最初のセッションに適した、小さく独立した変更です。

## セッションの構成

**セッション**は、エージェントとの会話を分離された専用ワークスペースで実行するものです。各セッションには専用の Git worktree とブランチがあるため、機能追加とバグ修正など複数の作業を同時に進めても変更が競合しません。サイドバーではセッションがリポジトリごとにまとまり、選択して切り替えられます。

セッションには、エージェントとの**会話**、ファイルの調査や編集を行うエージェントの**ツール アクティビティ**、差分付きの**変更ファイル一覧**が表示されます。

## セッションを開始して変更を依頼する

新しいセッションでプロジェクトを調べ、機能を実装します。前のレッスンで GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成します。

1. GitHub Copilot app を開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選びます。
4. プロンプト欄の下で **new working tree** と **Interactive** モードを選び、次のプロンプトを送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot は新しい worktree を作成してプロジェクトを調査し、必要なファイルを見つけてコードを作成します。

## 差分をレビューする

AI が生成した変更は、小さくてもマージ前にレビューします。

1. アプリ右上の **Toggle review panel** を選び、開いたパネルで **Changes** を選択します。

   ![Create PR ボタンの右にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム カードを表示する `GameCard.astro` に、評価がある場合は値を表示し、`starRating` が `null` の場合は `No rating yet` を表示するコードが追加されたことを確認します。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot の動作は確率的なため、実際のコードは例と異なる場合があります。似た動作になっていることを確認してください。

## 変更を確認する

ブラウザーで確認する前に、エージェントの自動チェック結果をレビューします。数値の `starRating` と `null` の場合の代替表示をテストしていることを確認してください。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストール要求は内容を確認してから承認します。

次に、Copilot に Web サイトを起動させ、ブラウザー キャンバスで UI を確認します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンではカスタム Canvas を確認して作成します。ここでは組み込みのブラウザー Canvas を使います。

1. 次のプロンプトを送信し、アプリの起動とブラウザー Canvas の表示を依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. アプリが起動し、Copilot app 内にブラウザー ウィンドウが表示されるまで待ちます。
3. 評価のあるゲーム カードに 5 段階評価が表示されることを確認します。
4. 次のプロンプトで、このセッションの開発サーバーを停止します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成できたので、コードを既存のコードベースにマージするプルリクエスト (PR) を作成します。

1. 右上の **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択して認証します。
3. リポジトリのポリシーにより関連 Issue が必要だと Copilot が伝えた場合は、その手順に従うよう確認します。
4. Copilot が PR を作成したら、チャットの上にある **PR** バブルを選び、レビューペインで内容を確認します。
5. 問題がなければ **Ready to merge** を選択します。
6. ダイアログで **Merge pull request** を選択してマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更を公開しました。このレッスンでは、次の操作を行いました。

- エージェント セッションを開始し、セッションの構成を確認しました。
- エージェントにゲーム カードの小さな変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を作成し、チェックをレビューして、自分でマージしました。

次は、[絞り込み機能の Issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを使う][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
