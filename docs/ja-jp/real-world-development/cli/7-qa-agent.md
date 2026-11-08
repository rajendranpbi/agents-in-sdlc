---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタム エージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上の絞り込み機能を確認しました。次は、明確な quality assurance (QA) プロセスを持つカスタム エージェントにこれらの機能をまとめます。

このレッスンでは、次の操作を行います。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な quality assurance (QA) プロファイルを作成して確認します。
- QA エージェントを選び、絞り込み機能の Issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テスト範囲、ブラウザーでの動作を一貫して確認したいと考えています。カスタム エージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタム エージェントとは

カスタム エージェントは、目的、命令、利用可能なツールを Markdown プロファイルで定義する、Copilot の特化版です。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチーム標準を示し、`quality-checks` スキルは繰り返し実行するチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA プロファイルは、これらを使って要件を評価し、結果を報告する方法を Copilot に指示します。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用できる QA プロファイルを Copilot に作成させます。プロファイルには、QA が行うチェックと守るべき境界を定義します。

1. Codespace に戻り、絞り込み機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに次のカスタム エージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

QA エージェントを使う前に、Copilot が意図したワークフローと制約をプロファイルに反映したことを確認します。これにより、検証だけを依頼したのに不完全なエージェントや範囲の広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須で、`name` は任意ですが、指定するとエージェントの表示名が明確になります。
3. 命令を読み、要件を起点とし、リポジトリの命令に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を報告し、実装コードを変更する前に確認を求め、コミットやプルリクエストの作成を行わないことを確認します。
5. 必要な責務や制約が欠けている場合は、続行する前に既定のエージェントへ修正を依頼します。

## Issue に対して QA を実行する

プロジェクトのエージェントは会話開始時に読み込まれます。プロファイルを作成した後、同じ絞り込み機能の会話を再開して QA を選択し、Issue と計画の内容を使えるようにします。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成直後は、利用可能なエージェントの一覧に表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能をレビューするよう依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を進める様子を確認します。
4. 作業が完了したら、出力されたレポートを読みます。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その結果を確認しました。このレッスンでは、次の操作を行いました。

- カスタム エージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な QA プロファイルを作成して確認しました。
- QA エージェントを選び、絞り込み機能の Issue に照らして結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートが揃いました。次は、[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタム エージェントを作成する][create-agents]
- [カスタム エージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
