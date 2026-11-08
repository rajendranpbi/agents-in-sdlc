---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム エージェント][previous-lesson] |
|:--|

agent mode の Copilot は、コードベースの調査、変更の提案、ファイル編集、コマンド実行を自律的に行います。VS Code の Copilot Chat では、ツール呼び出しや編集をリアルタイムで確認し、差分を採用または取り消し、追加プロンプトで作業を調整できます。

この演習では、アクセシビリティ エージェントの作業を監視して編集をレビューし、同じセッションにライト モードの追加を依頼します。

## シナリオ

[演習 4][previous-lesson]で使ったアクセシビリティ エージェントがハイコントラスト モードを実装中です。デザインチームから、既定のテーマが暗すぎると感じるユーザー向けにライト モードも追加してほしいと依頼がありました。新しい会話を始めず、進行中のセッションに依頼を追加します。

## Copilot の進行状況を監視する

Copilot Chat には、エージェントが調べたファイル、実行したコマンド、提案した編集が表示されます。編集は差分と **Keep** / **Undo** の操作とともに表示されます。

1. 演習 4 の Copilot Chat に戻ります。
2. チャットをスクロールし、ツール呼び出し、ファイルの読み書き、実行を求められたターミナル コマンドを確認します。
3. 編集差分ごとに、採用する場合は **Keep**、取り消して再試行させる場合は **Undo** を選びます。
4. チャット上部の **Files changed** で、このセッションで変更されたファイル全体を確認します。

> [!NOTE]
> ターミナル コマンドの実行などに許可が必要な場合、エージェントは **Allow** を表示して一時停止します。内容を確認し、許可してよいコマンドだけ承認してください。

5. ハイコントラスト モードの実装が完了し、変更の概要が表示されるまで待ちます。

## ライト モードを追加するようエージェントに指示する

同じ会話を続けると、エージェントは変更済みコンポーネント、スタイル、local storage の使い方などのコンテキストを保持します。

1. Copilot Chat に次のプロンプトを送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を確認し、ライト モードの更新を提案する様子を見ます。
3. 各差分を確認し、**Keep** または **Undo** を選びます。
4. 望まない方向に進んだ場合は、チャット入力欄の **Stop** で停止し、追加の説明を送ります。

> [!TIP]
> 進行中のセッションに指示を追加すると、最初からやり直すより速く調整できます。たとえば、CSS を簡潔にする、設定パネルの既存トグルにライト モードを統合する、両方のトグルにキーボード ショートカットを追加する、といった依頼ができます。

## 変更されたファイルをレビューする

コミット前に、エージェントが変更したすべての内容を確認します。

1. VS Code の **Source Control** ビューを開きます。
2. Astro コンポーネント、スタイル、関連テストなど、変更ファイルの一覧を確認します。
3. いくつかの差分を開き、カスタム エージェントのアクセシビリティ パターンが反映されていることを確認します。ARIA 属性、キーボード操作、セマンティック HTML、local storage による設定保持を確認します。
4. 問題がなければ **Source Control** パネルから変更をステージしてコミットします。次のレッスンで公開します。

## まとめと次のステップ

Copilot Chat agent mode で進行中の作業を監視し、各ファイルの編集を確認して採用または取り消し、コンテキストを保ったまま追加プロンプトを送信しました。次は[アクセシビリティの変更を PR として公開し][next-lesson]、このラボで作成した変更をレビューします。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [agent mode を使う][agent-mode]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
