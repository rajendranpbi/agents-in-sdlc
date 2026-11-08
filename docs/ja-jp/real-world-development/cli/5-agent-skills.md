---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、絞り込み機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発には多くの作業があります。これまで、コードを手動で検証し、命令ファイルを使ってプロジェクト標準に従わせてきました。テストや lint、継続的インテグレーション (CI) のほかの作業にはどう対応すればよいでしょうか。

こうした作業には、**エージェント スキル**が適しています。スキルを使うと、Copilot に適切な操作方法を伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの手順、実行可能なスクリプト、補助リソースが含まれます。エージェントは必要に応じてこれらを読み込みます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown ファイルには、スキル名と説明を定義する frontmatter、スキルの概要、呼び出すタイミングのガイダンスを記述します。フォルダーには、スキルが呼び出されたときに使用するスクリプトやその他のリソースを追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルには、プロジェクトの既存コマンドを使うため、`SKILL.md` のみが含まれています。

スキルは、チームで共有して再利用できるリポジトリ資産としてプロジェクトの `.github/skills` に配置できます。また、ユーザーのスキル フォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行に使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイル先頭の `name` と `description` を読みます。説明が、Copilot にスキルを呼び出すタイミングを伝えることを確認します。
3. 命令を読み、テストと lint の手順をどのように Copilot に示しているかを確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。まず、Copilot に 3 種類のチェックを実行するよう依頼します。

1. Interactive モードで絞り込み機能の会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した結果を確認できるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

スキルを変更したので、結果を確認します。Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどと同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- ユニットテスト、lint、型チェックを行う `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

変更内容は絞り込み機能のプルリクエスト (PR) に含めます。次は、[Playwright の Model Context Protocol (MCP) サーバーを使って][next-lesson] Copilot がサイトを直接操作できるようにします。

## スキルのその他の例

次のコミュニティの例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプトを含む `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
