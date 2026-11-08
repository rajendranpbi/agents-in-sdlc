---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー体験の改善や機能追加に関する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。この一元管理画面では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次の操作を行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

ハイコントラスト モードを依頼した後、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含めるのが適切です。エージェントに指示を追加し、ハイコントラストに加えてライト モードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインのタスク一覧を確認します。コードのドキュメント追加、ゲームを変更する API の生成、Web サイトのハイコントラスト モードなど、割り当てたタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーのセットアップ、タスク完了に向けた各種操作などが表示されます。

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選んだ方法によって異なります。

4. 右側に表示される PR ペインで、PR と変更されたファイルを確認します。

## cloud agent に指示を追加する

現在のタスクを確認したので、ハイコントラスト モードの作業中にライト モードも加えるよう依頼します。

1. ハイコントラスト モードのセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

   ![一元管理画面に表示されたアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが終わり、エージェントが作業を開始したことを示すメッセージが表示されるまで数分待ちます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェント ページで、作業中の cloud agent に指示を追加するダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に追加したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順を終えた後、新しい要望を作業に取り込みます。完了まで数分かかることがあります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、指示を追加するための中心的な場所であるエージェント ページを確認しました。進行中と完了済みのタスクを追跡し、実行内容をレビューして、進行中の作業に指示を加えられます。このレッスンでは、次の操作を行いました。

- エージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、作成されたプルリクエストを[反復的にレビューします][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復的に改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
