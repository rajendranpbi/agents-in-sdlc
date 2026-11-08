---
slug: ja-jp/real-world-development
title: "実践的な開発"
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使い、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復的に進めます。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として、ボードゲームのクラウドファンディング プラットフォームに取り組みます。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。フィルタリングやページネーションなどの機能開発に加え、アクセシビリティやコーディング標準の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、さまざまな作業環境で利用できます。開発方法に合った環境を選んでください。各ワークショップは個別にセットアップし、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright の Model Context Protocol (MCP)、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を案内して進行状況を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat の agent mode、MCP サーバー、カスタム エージェントを扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
