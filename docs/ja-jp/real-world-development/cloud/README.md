---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot をクラウドで非同期に動作させられます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調べて変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

このワークショップでは、まず cloud agent が従うカスタム命令を追加し、GitHub Issue を割り当てて実装を依頼します。カスタム エージェントを確認してそのアプローチを調整し、agents ダッシュボードからセッションの進行状況を監視して指示を追加します。最後に、作成されたプルリクエストをレビューし、結果を反復的に改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. カスタム命令][ex1] | コンテキスト | cloud agent が従うカスタム命令を追加する |
| [2. Cloud agent][ex2] | 非同期エージェント | Issue を Copilot cloud agent に割り当てる |
| [3. カスタム エージェント][ex3] | 特化型エージェント | カスタム エージェントを確認して使用する |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して指示を追加する |
| [5. 反復][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して次の手順を選ぶ |

## 前提条件

このワークショップを始める前に、次のものを準備してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** プランがある GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、Model Context Protocol (MCP)、コードレビュー、Copilot CLI の機能が含まれています。そのため、どの harness も完了できます。

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Business または Enterprise では、管理者が組織で有効にする必要があります。

> [!NOTE]
> この harness では Model Context Protocol (MCP) を扱いません。Copilot で MCP サーバーを使う方法については、[CLI harness][cli] または [VS Code harness][vscode] を参照してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[student-plan]: https://github.com/education/students
[cli]: ../cli/
[vscode]: ../vscode/
[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
