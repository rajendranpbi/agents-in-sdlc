---
title: "演習 5: GitHub Copilot の作業を反復的に改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

これまでに GitHub Copilot に、コードのドキュメント追加、関連ゲーム機能の作成、ハイコントラストとライト モードによるアクセシビリティ改善を依頼しました。コードを確認し、必要に応じて Copilot にフィードバックを送ります。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チームでコードレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能のプルリクエストを確認します。

## Copilot cloud agent とセキュリティ

cloud agent は非同期に作業するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取り権限を持ち、書き込みは作業用ブランチに限られます。
- cloud agent は GitHub Actions 内に一時的な環境を作成して実行します。
- GitHub Actions ワークフローの実行には、人による承認が必要です。
- MCP サーバーを含む外部リソースへのアクセスは、既定で制限されています。

## 生成されたドキュメントをレビューする

最初に、コードへドキュメントを追加するために Copilot cloud agent が作成した PR を、GitHub.com の標準的な PR 画面で確認します。

> [!NOTE]
> Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot の外部リソースへのアクセスは既定で制限されているため、これは想定された動作です。必要であれば、[cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Pull Requests** を選択します。
3. **Add missing documentation** などのタイトルの PR を開きます。Copilot が作業中の場合、タイトルに **[WIP]** が含まれます。完了するまで待ちます。
4. **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TSDoc コメントやその他のドキュメントを確認します。変更内容は状況によって異なります。Exercise 2 で確認した命令ファイルに基づく TSDoc コメント、TypeScript の規約、コメント ヘッダーを探します。
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認します。
8. 承認待ちの場合は **Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローが実行され、単一 Astro アプリのプロジェクト チェックが成功することを確認します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

PR のコメントやコードのインライン コメントで、チーム メンバーに依頼するように Copilot に変更を依頼できます。Copilot はコメントを確認して、更新用のセッションを開始します。結果は一定ではないため、具体的な文面を指定することはできません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に役割を説明するコメント ヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. PR またはコードのインライン コメントに、提案のいずれか、または希望するドキュメント改善を記述し、**@copilot** をメンションします。
2. **View Session** を選択し、Copilot が更新する様子を確認します。更新用の新しいセッションが開始されます。
3. **Back to pull request** を選択すると PR に戻れます。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業完了後、PR に新しいコミットがあることを確認します。
5. **Files changed** タブで変更をレビューします。必要に応じて反復し、問題がなければドラフトをレビュー可能にして `main` にマージします。

![ドラフト PR をレビュー可能な状態にする画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブで生成されたコードを確認します。
5. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローの承認待ちを示す表示があるか確認し、承認待ちなら **Approve and run workflows** を選択します。
7. チェック欄でワークフローが実行され、単一 Astro アプリのチェックが成功することを確認します。
8. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。ブランチ名を Copilot が作成したものに置き換えます。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

必要なら PR の **Conversation** タブにコメントを追加し、表示する関連ゲームの数やコメント ヘッダー、TSDoc コメントなどの変更を Copilot に依頼します。Issue をカスタム命令の変更前に割り当てたため、その命令がまだ適用されていない場合があります。

## アクセシビリティ機能をレビューする

カスタム アクセシビリティ エージェントが実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 で追加したライト モードが含まれるはずです。

1. GitHub.com のリポジトリを開きます。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。Copilot が作業中なら **[WIP]** が含まれるため、完了まで待ちます。
4. **Files changed** タブで変更を確認します。
5. 特に次の項目を確認します。
   - モード切り替え用の UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラスト モードとライト モードの CSS またはスタイル
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript
6. 問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローの承認待ちを示す表示があるか確認し、承認待ちなら **Approve and run workflows** を選択します。
8. チェック欄でワークフローが実行され、単一 Astro アプリのチェックが成功することを確認します。
9. **任意:** Codespace でブランチをチェックアウトし、アプリを起動して、ハイコントラストとライト モードの切り替えをテストします。ページを再読み込みした後も設定が保持されることを確認します。

カスタム アクセシビリティ エージェントが、アクセシビリティのベスト プラクティスに沿うよう Copilot を導いたことを確認します。改善点があれば、ほかの PR と同様にコメントで **@copilot** をメンションします。

## 任意の演習 — 委任を続ける

cloud agent に実際のバックログ項目を割り当て、別の作業に取り組む方法を試します。たとえば、次の Issue を作成して割り当てられます。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent harness を完了しました。これらのレッスンでは、次の操作を行いました。

- リポジトリのカスタム命令を確認し、cloud agent の出力への影響を把握しました。
- Issue を Copilot cloud agent に割り当て、環境構築、計画、非同期の実行を確認しました。
- アクセシビリティ用のカスタム エージェントを作成して使い、ハイコントラストとライト モードを追加しました。
- Copilot agents ページを一元管理画面として使い、進行中のアクセシビリティ セッションを監視して指示を追加しました。
- Copilot の PR をレビューし、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent harness を完了しました。ほかの環境のワークショップでは、ここで学んだ内容を別の方法で実践できます。

- 🖥️ **[VS Code harness](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を試します。
- 💻 **[CLI harness](../../cli/)** — Plan、Autopilot、スキル、QA エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューして公開します。

自分のリポジトリでは、次の作業も試せます。

- リファクタリングやテスト範囲の拡充を cloud agent に割り当てる。
- 別の分野向けのカスタム エージェントを作る。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
