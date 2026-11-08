---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue から絞り込み機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って作業できます。このレッスンから数回にわたり、Issue を基に機能を作成し、コードの妥当性と機能の動作を確認して、プロジェクトにマージするまでの一般的なエージェント駆動開発を進めます。

> [!NOTE]
> 機能開発を進める間は、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため、手順を簡略化します。

このレッスンでは、次の操作を行います。

- GitHub Issue から Copilot との新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- ポート フォワーディングしたブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや発行元でゲームを絞り込む必要があります。バックログの Issue には機能が記されていますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な点があります。Plan モードでその方針を決めてから、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに加えても、基本は変わりません。一般的な開発フローは次のとおりです。

1. 実装内容を記述した Issue から始めます。
2. 実装計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーション (CI) が成功したら、コードをマージします。

> [!NOTE]
> 具体的な手順はチームや組織によって異なりますが、多くは上記の流れを基にしています。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビュー プロセスを通るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: 開発者とエージェントが協力します。エージェントは変更を提案し、入力を待ってから次に進みます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルは編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7`（または同様の番号）が表示されます。`#` を使うと、GitHub 上の Issue や PR を会話のコンテキストに追加できます。

## 絞り込み機能を計画する

計画を立てると、機能の実装方法やタスクの進め方を Copilot に任せる前に決められます。複雑な作業では、まず計画を立てることをお勧めします。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプト下のモード表示が **Plan** であることを確認します。
2. 直前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、最適な方法を判断して計画を作成します。

3. 絞り込みの動作について質問された場合は、希望に基づいて回答します。決まった正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 決めた方針と一致しない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を確認したら、実装に進みます。Autopilot は、タスクが完了したと判断するまで Copilot が作業を反復できるようにします。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みバージョンで同様のラベルの選択肢を選びます。
2. プロンプト下のモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると、すぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると報告された場合は、チェック完了と判断する前に環境の問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様、マージする前にレビューします。コードを確認し、サイトを起動して動作も確かめます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、絞り込み機能の実装とテストを確認します。
3. 結果を Issue および計画で決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードの準備ができたら、Codespaces のポート フォワーディングを使ってサイトを開き、機能を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリ、発行元、およびその組み合わせで絞り込みを試します。
4. リセット時と検索結果がない場合の動作が、Issue と決定した内容に一致することを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて機能を構築し、レビューしました。このレッスンでは、次の操作を行いました。

- GitHub Issue から Copilot との新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- ポート フォワーディングしたブラウザーで機能を手動検証しました。

次は、[カスタム命令を使って][next-lesson]コードが文書化された規約に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
