---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。[命令ファイル][instruction-files]を使うと、Copilot にコードの内容だけでなく、生成方法も伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスに応じた命令ファイルが Copilot に届く仕組みを確認します。
- 既存の命令でフィルタリング用データ スライス (publishers ヘルパー) を生成します。
- `.github/copilot-instructions.md` に新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードへの影響を確認します。
- 命令の更新とフィルタリングの基盤を `main` にコミットし、次の演習に備えます。

> [!CAUTION]
> 生成コードが標準の一部に従わない場合もあります。Copilot の出力は一定ではありません。ここでは、命令を更新した後の傾向の変化を確認します。文字単位で同じ出力になることを期待しないでください。

## 命令ファイル

### シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。データ レイヤーには必ずユニットテストを作成し、UI はダーク モードで現代的な見た目にします。コードには TSDoc コメントを追加し、各ファイルの先頭には役割を説明するコメントを置きます。命令ファイルで、Copilot にこれらを伝えます。

### カスタム命令

カスタム命令は、コーディング スタイルや要件のコンテキストを Copilot に提供します。リポジトリ全体、または特定のファイルやタスクに命令を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られます。技術スタック、プロジェクトの概要、ベスト プラクティスなど、広く関係する情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの言語、UI コンポーネントやテストなどのタスクに応じたガイドラインを記述します。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの命令と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成時に使います。AGENTS.md や CLAUDE.md など、別の標準もサポートされています。

### 命令ファイルを管理するためのベスト プラクティス

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準などを記述します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ レイヤーなどのファイルやタスクに使います。
- 自然な言葉で明確に記述し、コードの良い例と悪い例を示します。

命令ファイルを作成する唯一の正解はありません。プロジェクトに合わせて試し、改善します。

> [!TIP]
> このプロジェクトには [UI 更新用][ui-instructions]や [Astro 用][astro-instructions]など、さまざまな命令ファイルがあります。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` を使い、Copilot に作成を支援させることもできます。テンプレートは [awesome-copilot][awesome-copilot]で確認できます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの命令ファイルを確認する

変更前に、スターター プロジェクトに含まれる命令を確認します。

1. Codespace に戻り、`.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目します。
3. `.github/instructions` を確認し、Astro、Drizzle、テストなどの命令を探します。
4. `.github/instructions/unit-tests.instructions.md` を開き、`applyTo` の glob が `**/*.test.ts` などの TypeScript テストに適用されることを確認します。
5. `.github/instructions/drizzle.instructions.md` の末尾で、他の命令ファイルやプロジェクト ファイルへの相対リンクを確認します。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** には、コード内のドキュメントを求める規則がまだありません。

## 変更用ブランチを作成する

変更を分離するため、前の演習の Codespace に戻り、新しいターミナルを開いてブランチを作成します。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
2. 次のコマンドを実行します。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

変更前後を比較するため、まず既存の命令のままプロンプトを実行します。

> [!TIP]
> **Copilot Chat を開く**
>
> Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。

1. 開いているエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェント選択欄で **Agent** を選びます。

   ![Chat ビューの agent picker](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認し、ヘルパーとテストなどのコードを変更する様子を見ます。
6. Drizzle 命令に基づき、`db` クライアントを最初の引数に取り、型付きの発行元配列を返す関数が提案されていることを確認します。
7. 提案されたコードに TSDoc コメントとファイル先頭のコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくてもコメントを追加する場合があります。その場合も問題ありません。命令の更新後に一貫性が高まることがこの演習の目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` にドキュメント標準を追加します。

1. ファイルを開き、27 行目付近の **Code formatting requirements** を確認します。
2. 既存の標準の直後に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

## 同じプロンプトを再実行して変化を確認する

更新した命令が Copilot に反映されるか、同じプロンプトで確認します。

1. Copilot Chat で **New Chat** を選びます。
2. `src/lib/publishers.ts` を開き、Copilot の対象ファイルにします。
3. 前と同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. ファイル先頭に役割を説明するコメントが追加され、関数に目的、`db` 引数、戻り値を説明する TSDoc が追加されていることを確認します。提案内容は例と完全一致しない場合があります。

## 命令とブランチをコミットしてプッシュする

命令とヘルパーの変更をソース管理に保存します。

1. 新しいターミナルを開き、`git status` で変更を確認します。
2. `.github/copilot-instructions.md`、`src/lib/publishers.ts`、および必要なテストをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

3. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

GitHub ツールを使って、バックログの命令更新 Issue に関連付けた PR を作成し、マージします。

1. Codespace の Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押し、**Agent** を選択します。
3. 次のプロンプトを送信します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成する様子を確認します。GitHub 操作の許可を求められたら、内容を確認してから **Allow** を選択します。
5. PR 作成後、次のプロンプトでマージしてローカル ブランチを更新します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作やシェル コマンドの許可を求められたら、内容を確認してから **Allow** を選択します。

## まとめと次のステップ

既存の命令での生成結果を確認し、新しいリポジトリ標準を追加して同じプロンプトを再実行しました。命令の変更とヘルパーの基盤をコミットして `main` にマージしました。次は、[agent mode で][next-lesson]命令を使い、コードベース全体に機能を追加します。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot のカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
