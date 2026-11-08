---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景がある場合は、その情報を参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に何を実装するかだけでなく、どのように実装するかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルで、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の harness とは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムの比較が難しいためです。この harness の後半で、cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認します。

## 命令ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- データ レイヤーには必ずユニットテストを作成します。
- ユーザー インターフェイス (UI) はダーク モードで、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

命令ファイルを使い、Copilot がこれらの慣行に沿って作業できるようにします。

### カスタム命令

カスタム命令は、コーディング スタイルや要件のコンテキストを Copilot に伝えます。リポジトリ全体、または特定のファイルやタスクに命令を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベスト プラクティスなど、広く関係する情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントや新しいテストなどのタスクごとにガイドラインを設定します。

> [!NOTE]
> 統合開発環境 (IDE) では、命令ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の命令ファイルもサポートしています。

### 命令ファイルを管理するためのベスト プラクティス

スターター プロジェクトの例を確認し、命令ファイルを管理する方法を学びます。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ レイヤーなどのファイルの種類や、特定のタスクに使います。
- 自然な言葉で明確に記述し、コードの良い例と悪い例を示します。

命令ファイルを作成する唯一の正解はありません。プロジェクトや環境に合わせ、試しながら改善します。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した命令ファイルを用意します。このプロジェクトには [UI 更新用][ui-instructions]や [Astro 用][astro-instructions]などがあります。
>
> Copilot に命令ファイルの作成を支援させることもできます。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、各環境で手順が異なります。
>
> テンプレートを探す場合は、命令ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

プロジェクトには、コアとなる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。これらは Copilot とのすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` がリポジトリ ルートからの glob として適用範囲を定めていることを確認します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルに適用されます。
5. ユニットテスト作成に関する命令を確認します。
6. `.github/instructions/drizzle.instructions.md` の末尾まで読み、`unit-tests.instructions.md` など他の命令やプロジェクト ファイルへのリンクを確認します。これらのパスはリポジトリ ルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内のドキュメントはまだ求めていません。次に TSDoc コメントとファイル先頭のコメント ヘッダーを追加します。

## リポジトリ全体の標準を追加する

`.github/copilot-instructions.md` にドキュメント標準を追加します。cloud agent は、この harness の後半で割り当てる Issue に取り組むときに、この命令を読み込みます。

1. Codespace のターミナルで、作業用のブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き直します。
3. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準には、生成されたヘルパーにドキュメント コメントがなかった理由となる規則がまだありません。
4. 既存の標準の直後に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. `copilot-instructions.md` を保存します。

> [!TIP]
> 命令ファイルは、リポジトリ全体の標準には `.github/copilot-instructions.md`、特定の言語やタスクには `*.instructions.md` を使います。今回追加したドキュメント コメントの規則は、リポジトリ全体に適用するため、前者に記述します。

## 命令の更新をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの命令ファイルを読み込みます。次の演習では `main` からブランチを作るため、更新を `main` に反映します。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの命令を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。ここでは実際のプロジェクトに近い手順を使っています。

## まとめと次のステップ

このプロジェクトで Copilot が命令ファイルからコンテキストを読み取る仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この標準が、cloud agent の作成するプルリクエストに反映されることを後の演習で確認します。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
