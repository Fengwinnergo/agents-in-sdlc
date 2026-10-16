---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認し、ドキュメント作成の標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。作業の進め方に決まりがある場合は、Copilot がそのガイダンスを利用できるようにします。[命令ファイル][instruction-files]には、作成したいコードだけでなく、コードの構成方法も記述します。絞り込み機能を作成したので、Copilot が参照した命令を確認し、ドキュメント作成の標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの命令とパス別の命令がエージェントに渡されるしくみを確認する。
- コーディング標準を守るために命令ファイルを更新する。
- 命令ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys では、開発方法に関するガイドラインと要件を定めています。

- コメントにはコードを言い換えるのではなく、意図や自明でない判断を記載する。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記載する。`db` 引数を注入できる場合は、その説明も含める。
- 再利用可能な Astro コンポーネントには `Props` の仕様を記載し、関連コードが変更されたときはコメントも最新に保つ。
- 既存の書式と lint のガイダンスを維持する。

命令ファイルを使って、Copilot がこれらの実践に沿ってタスクを実行できるようにします。

## 命令ファイル

カスタム命令を使うと、コンテキストや好みを Copilot に伝え、コードのスタイルや要件を理解しやすくできます。これは、関連性の高い提案やコード スニペットを生成するための強力な機能です。コーディング規約、ライブラリ、コメントの付け方などを指定できます。リポジトリ全体に適用する命令と、特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の命令ファイルです。Copilot へのチャットや CLI のリクエストの多くに関係する、プロジェクト全体のコンテキストを記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイルの種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> その他の命令形式とそのサポート状況は、環境によって異なります。形式を使う前に、[カスタム命令のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには命令ファイルが用意されています。変更の効果を確認できるよう、まず既存のファイルを見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この命令は、リポジトリ内のすべての Copilot とのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令の適用対象を指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルやプロジェクト ファイルへのリンクを確認します。大きな命令セットを小さなファイルに分けて再利用し、参考になる例を指定できます。

## チームのガイダンスに合わせて命令ファイルを更新する

用意されているファイルには不足があります。コアの `copilot-instructions.md` を変更して、新しく生成する TypeScript に TSDoc コメントを付けるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを探します (35 行目付近です)。
2. 最後の箇条書きに次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。命令を更新したら、絞り込みの会話を終了して再開します。これにより、機能のコンテキストを保ちながら、新しいガイダンスを読み込めます。

1. 絞り込みの会話が開いている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められたら絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを送信して、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルをどのように使うかを確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの命令とパス別の命令がエージェントに渡されるしくみを確認しました。
- コーディング標準を守るために命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
