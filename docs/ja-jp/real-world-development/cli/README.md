---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエストの自動化を学びながら、Tailspin Toys に 2 つの変更を加え、検証して公開します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、GitHub Copilot をエージェント型コーディング アシスタントとしてターミナルで利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカル エディターに切り替えずに作業を続けられます。

このワークショップでは、Tailspin Toys の一連の作業を通して、次の操作を行います。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本を確認します。
2. 星評価の変更に取り組み、ブラウザーで確認して、最初のプルリクエスト（PR）を手動でマージします。
3. 絞り込み機能の Issue を基に Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューします。
4. リポジトリの命令を更新し、絞り込み機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright の Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込み機能を確認します。
7. quality assurance (QA) カスタム エージェントを作成して使い、要件、テスト範囲、検証結果を確認します。
8. 絞り込み機能全体をレビューし、Agent Merge を使ってその PR をマージします。
9. コンテキスト、モデル、共有、任意のクラウド委任に役立つスラッシュ コマンドを確認します。

ワークショップでは、星評価の PR と、命令の更新、スキルの更新、QA プロファイル、テストを含む絞り込み機能の PR の 2 つを作成します。絞り込み機能と品質確認の作業では、同じ会話とブランチを使い、各ツールでの作業を積み重ねます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本を確認する |
| [2. 星評価を追加する: まずは小さな改善][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を行い、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム命令で Copilot を導く][ex4] | コンテキスト | 既存の命令を確認して更新し、絞り込み機能に適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 再利用可能なチェック | 既存スキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | CLI で MCP を構成し、絞り込み機能の動作を確認する |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門エージェントを作成して選び、最終的な検証結果を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して Agent Merge を使う |
| [9. GitHub Copilot CLI のスラッシュ コマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、参考資料を確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づくエージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップを始める前に、次のものを準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナルまたはコマンドラインの基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
