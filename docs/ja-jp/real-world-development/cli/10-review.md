---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連の作業を通して、GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意された絞り込み機能の Issue を見つけました。
- 星評価を追加し、ポート フォワーディングしたブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム命令でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- quality assurance (QA) カスタム エージェントを作成して選び、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込み機能全体をレビューし、Agent Merge による PR のマージを承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任のスラッシュ コマンドを確認しました。

## 公開した変更

ワークショップには、それぞれ `main` から作成したブランチで進める 2 つの PR があります。

1. **星評価**: 既存の `starRating` と未評価状態をゲーム カードに表示します。
2. **絞り込み機能と品質確認**: 絞り込みを実装し、命令を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成し、関連テストを追加します。

絞り込みの計画から PR 作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告します。QA エージェントは、それらの結果と要件およびテスト範囲のレビューをまとめます。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを承認する前にコードと根拠を確認し、変更後は影響する検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し作業を記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を考えるには **Plan**、範囲の絞られた変更でやりとりを続けるには **Interactive**、明確なタスクを進めるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを終えたので、次の CLI 機能も試せます。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けることが一番です。本番コード、趣味のコード、長く作りたいと思っていた小さなアプリなどで試し、学んだことをチームと共有しましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
