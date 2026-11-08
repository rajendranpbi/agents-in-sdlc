---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]は、リポジトリの `.github/agents` にある Markdown ファイルで定義する、特定のタスクや分野に特化したアシスタントです。命令、ベスト プラクティス、コーディング パターン、専門知識を指定できます。たとえば、アクセシビリティ、セキュリティ、テスト用のエージェントを作成できます。

エージェント ファイルには `name` と `description` を含む YAML frontmatter と、動作や専門知識を記述する Markdown プロンプトが必要です。ユーザー共通のエージェントは `~/.copilot/agents` に置けます。

### カスタム エージェントとエージェント スキル

カスタム エージェントは作業を行う担当者、[スキル][agent-skills-concept]は作業に使うツールと考えられます。カスタム エージェントは独自のコンテキストを持ち、スキルや他のエージェントを調整します。この演習のアクセシビリティ エージェントはサイトを確認して更新し、必要に応じてテストや PR 用のスキルを呼び出せます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるクラウドファンディング サイトを目指しています。現在のダーク テーマは文字と背景のコントラストが不足して読みにくいというフィードバックを受け、切り替え可能なハイコントラスト モードを追加します。アクセシビリティ エージェントに機能を実装させます。

この演習では、既存のアクセシビリティ エージェントを確認し、Copilot Chat で使ってハイコントラスト モードを実装します。

## アクセシビリティ エージェントを確認する

アクセシビリティ作業用のエージェントを確認し、作業用ブランチを `main` から作成します。

1. Codespace に戻り、ターミナルを開いてブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. `.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. アクセシブルなサイト生成の責務、ベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある問題を確認します。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat では、`.github/agents` のエージェントを agent picker から選べます。既存の会話のコンテキストを引き継げるよう、新しい会話を始めます。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace の Copilot Chat パネルを開き、**New Chat** を選びます。モードとモデルの選択は演習ごとに異なります。

1. Chat ビューの agent picker で **Agent** を選びます。
2. agent picker を開き、**Accessibility agent** を選びます。
3. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

4. Copilot がコードベースを調べて変更を提案し、ファイルを編集する様子を確認します。各編集はファイル パスと差分とともに Chat に表示されます。
5. **Files changed** の表示で、変更されたファイルを確認します。Astro コンポーネント、CSS、関連テストなどの更新に数分かかることがあります。

この作業は次の演習で確認し、途中で指示を追加します。

## まとめと次のステップ

カスタム エージェントの役割を確認し、Copilot Chat の agent mode でアクセシビリティ エージェントを使いました。次は、エージェントの作業を[監視して指示を追加し][next-lesson]、同じセッションにライト モードも依頼します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
