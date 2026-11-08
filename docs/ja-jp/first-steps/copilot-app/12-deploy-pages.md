---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを構成し、プルリクエストを確認して、公開された Space Quiz を検証します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages を使って Space Quiz を公開しましょう。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューし、サイトが公開されたことを確認するのは開発者の役割です。

このレッスンでは、次の操作を行います。

- Copilot に GitHub Pages のデプロイを構成するよう依頼します。
- マージする前にワークフローとプルリクエストを確認します。
- デプロイの進行状況を追跡し、公開されたサイトを検証します。

## シナリオ

Space Quiz は GitHub リポジトリにありますが、まだ手軽に試せる場所がありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーや実行時の依存関係を追加せずに、公開 Uniform Resource Locator (URL) から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストがマージされた状態、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の実行、および yourname.github.io/space-quiz/ で公開された Space Quiz を示す 3 段階の図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトは、プルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更をエージェントに依頼します。続行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローを確認してデプロイを検証する

ワークフローと公開サイトを順に確認し、エージェントの報告だけに頼らず、デプロイが成功したことを確かめます。

1. README がまだない場合にのみ追加されていることを確認します。
2. ワークフローのビルド手順とデプロイ手順を含め、プルリクエストの変更を確認します。
3. 必須のプルリクエストチェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行を追跡し、成功するまで確認します。
5. 報告された URL を開き、Space Quiz が表示されることを確認します。
6. エージェントが Pages の有効化やプルリクエストのマージを実行できない場合は、提示された具体的な手順に従います。デプロイが成功し、公開サイトが読み込まれるまで完了と見なさないでください。

## まとめと次のステップ

このレッスンでは、次の操作を行いました。

- Copilot に GitHub Pages のデプロイを構成するよう依頼しました。
- マージする前にワークフローとプルリクエストを確認しました。
- デプロイの進行状況を追跡し、公開されたサイトを検証しました。

Space Quiz を公開し、エージェントの報告に頼らずサイトがデプロイされたことを確認しました。続けて[振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
