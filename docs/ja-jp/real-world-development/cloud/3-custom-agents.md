---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、専用の命令、ベスト プラクティス、コーディング パターン、分野固有の知識を与えます。チームの専門知識を再利用可能なエージェントにまとめられます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` に置く Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、命令を記述する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。違いを簡潔に表すと、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルやほかのエージェントを組み合わせて作業を進めます。この演習では、アクセシビリティ エージェントがサイトをガイドラインに照らして確認し、更新します。その作業の一部として、プルリクエストのワークフローやテスト実行用のスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。AI を使うほかの作業と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次の操作を行います。

- カスタム エージェントの定義方法を確認します。
- タスクをカスタム エージェントに割り当てます。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のフィードバックでは、現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいという意見がありました。そこでデザインチームは、ユーザーが切り替えられるハイコントラスト モードを求めています。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

## アクセシビリティ エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。Copilot を導く内容を確認します。

1. Codespace に戻り、`.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトを生成する際の主な責務、ベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある問題が記載されています。

## Issue を作成して割り当てる

エージェントの一元管理画面から、cloud agent にタスクを割り当てたり、進行状況を監視したり、指示を追加したりできます。まず、ハイコントラスト モードの作成を依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

   ![カスタム エージェントと Accessibility が選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。カスタム エージェントを使うと、チームの専門知識や標準を再利用可能な形にまとめ、特定の作業を Copilot に任せられます。この演習では、次の操作を行いました。

- カスタム エージェントの定義方法を確認しました。
- タスクをカスタム エージェントに割り当てました。

Copilot がハイコントラスト モードを実装している間に、次はエージェントのセッションを[監視して指示を追加します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
