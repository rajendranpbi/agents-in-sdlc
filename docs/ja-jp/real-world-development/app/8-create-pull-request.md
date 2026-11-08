---
title: "レッスン 8 - 機能の PR の作成とマージ"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストを1つのブランチに保存しました。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの別々の PR に分けます。ワークショップを円滑に進めるため、フィルター機能と品質に関するワークフロー全体を1つのセッションとブランチで進め、そのすべての作業をこの PR にまとめました。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを確認します。
- 機能全体の PR と検証結果を確認します。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認します。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使用して機能の計画、実装、検証を行いました。Tailspin Toys は、マージの承認を開発者の管理下に置きながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge の概要

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするまでの作業を自動化します。有効にすると、セッションが PR を確認し、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど、マージを妨げる問題に対処します。GitHub が許可するとマージし、バックグラウンドで動作します。アプリを再起動しても継続し、PR がマージされると自動的に無効になります。

ここまでは、自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、コードの編集やマージには引き続き明示的な承認が必要です。マージを許可する前に、許可される操作と作業内容をレビューしてください。

## Agent Merge で PR を管理する

コードを作成してレビューしたので、Agent Merge に PR プロセスを管理させます。

1. agent picker で **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選びます。ボタンの表示が **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択してプロセスを開始します。ドロップダウンの選択だけでは開始されません。

Agent Merge は次の作業を行います。

- タイトルと説明を付けて PR を作成します。
- Issue からセッションを開始した場合は、説明本文で関連 Issue に言及します。
- ターゲット ブランチに対してリベースし、マージ競合に対処します。
- CI を監視し、すべてのチェックが成功するようにします。
- 開発者や Copilot code review のフィードバックを確認し、コメントに対応します。
- すべてのチェックが成功した後、必要に応じて自動マージします。

すべてのチェックが成功したら、Agent Merge に PR をマージさせます。

5. **Agent Merge** の横にあるドロップダウンを選択します。
6. PR の差分と検証結果を確認してから、マージ権限を許可します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. エージェントに明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 進行状況を追跡し、報告されたブロッカーに対処します。続行前に PR の状態が **Merged** であることを確認します。マージ権限が有効になったことやエージェントが実行中であることだけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前にそれらの阻害要因を解消してください。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次の作業を行いました。

- Agent Merge の概要と、マージのライフサイクルを自動化する仕組みを学習した。
- 機能の PR 全体と検証の証拠を確認した。
- レビュー後にのみ Agent Merge を承認し、PR がマージされたことを確認した。

次は、[既存のキャンバスを使用してトリアージキャンバスを作成し][next-lesson]、エージェントと一緒に作業を確認、計画、視覚化するための、より豊かな方法を学びます。

## リソース

- [GitHub Copilot app での Issue と pull request の管理][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app