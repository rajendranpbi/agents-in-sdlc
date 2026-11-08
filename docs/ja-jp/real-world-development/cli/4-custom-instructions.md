---
title: "レッスン 4 - カスタム命令で Copilot を導く"
description: "リポジトリの命令を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合、そのガイダンスを Copilot が参照できるようにします。[命令ファイル][instruction-files]には、求めるコードだけでなく、その構成方法も記述できます。絞り込み機能を実装したので、Copilot が参照した命令を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパスに応じた命令ファイルがエージェントに届く仕組みを確認します。
- コーディング標準に従うよう命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発手法に関するガイドラインがあります。

- コメントにはコードの繰り返しではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記述します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディング スタイルや要件のコンテキストを Copilot に伝えられます。リポジトリ全体、または特定のファイルやタスクに命令を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべての Copilot リクエストに送られる命令ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、言語、ファイルの種類、タスク別のガイドラインを記述します。

> [!NOTE]
> ほかの命令形式とそのサポート状況は環境によって異なります。形式に依存する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトに含まれる命令を確認し、変更を加える前の状態を把握します。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディング ガイダンスを確認します。この命令は、リポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの命令ファイルを確認します。
5. `.github/instructions/unit-tests.instructions.md` を開き、適用対象のファイルを決める `applyTo` フィールドを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト ファイルへのリンクを確認します。命令を小さな再利用可能ファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルには、生成する TypeScript に TSDoc コメントを追加する標準がありません。コアの `copilot-instructions.md` を更新します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保ったまま新しい命令を読み込めるよう、会話を終了して再開します。

1. 絞り込み機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリ ルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   メッセージが表示されたら、絞り込み機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがあり、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを読み込む仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次の操作を行いました。

- リポジトリの命令とパスに応じた命令ファイルがエージェントに届く仕組みを確認しました。
- コーディング標準に従うよう命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実施します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
