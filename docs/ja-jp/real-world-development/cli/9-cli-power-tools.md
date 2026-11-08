---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト、モデル、セッション共有、任意のクラウド委任を管理するスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

GitHub Copilot CLI には、多くのスラッシュ コマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を確認します。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了しました。次はコンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルのコンテキスト ウィンドウの上限に近づくことがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュ コマンドを使って、自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリ ルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域ごとのコンテキスト量を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に圧縮します。タイミングを指定する場合は `/compact` を使います。関係のないタスクに切り替えて会話を新しくする場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、モデルを一覧表示して選択できます。

1. `/model` を入力します。
2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` は、セッションを Markdown または HyperText Markup Language (HTML) ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown に出力する `/share file` など、用途に合った出力先を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの主要な作業は完了しました。Copilot CLI には、大きなタスクで使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期で作業し、プルリクエストを開くことがある Copilot cloud agent にタスクを送信できます。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく確認するには、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュ コマンドを使って Copilot CLI を構成し、セッションを共有し、内部の状態を確認しました。このレッスンでは、次の操作を行いました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を確認しました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

ほかにも多くのスラッシュ コマンドがあります。次は、[学んだ内容を振り返り][next-lesson]、Copilot CLI の学習を続ける方法を確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
