---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進める必要がある場合は、Copilot がそのガイダンスを利用できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。重要な方法の 1 つが[命令ファイル][instruction-files]です。コード生成の方法に関するガイダンスを与えます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス別の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わるしくみを確認する。
- 現在の命令を使って絞り込み機能用の最初のデータ スライス (publishers ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認する。
- 次の演習で Copilot が更新後のガイダンスを使えるよう、命令の更新と絞り込みスライスを `main` にコミットする。

> [!CAUTION]
> 生成されたコードが設定した標準に沿わないことがあります。Copilot は確率的に動作するためです。この演習では、命令を更新する前後で挙動がどう変わるかを確認します。出力が一字一句一致することを目指すものではありません。

## 命令ファイル

### シナリオ

Tailspin Toys には開発方法に関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- UI はダーク モードにし、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを使用します。
- 各ファイルの先頭に、そのファイルの機能を説明するコメント ブロックを記載します。

命令ファイルを使って、Copilot がこれらの実践に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。関連性の高い提案やコード スニペットを生成するための強力な機能です。コーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する命令と、特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信されます。Copilot へのチャットや CLI のリクエストの大半に関連する、技術スタック、作成するものの概要、ベスト プラクティス、全体的なガイダンスなどを記載します。
- `.github/instructions/*.instructions.md` ファイルには、言語、ファイルの種類、タスク向けのガイドラインを記載します。TypeScript や Astro、UI コンポーネントの作成、単体テストの作成などに使用できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイル管理のベスト プラクティス

命令ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関わる内容を記載します。
- `*.instructions.md` には、単体テスト、Astro コンポーネント、データ レイヤーなど、ファイル種別や特定のタスク向けのガイダンスを記載します。
- 自然な言葉で、明確なガイダンスとコード例を示します。

命令ファイルの作成方法は 1 つではありません。実験を通じて、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した命令ファイルを用意することをお勧めします。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業に適用されるファイルがあります。
>
> Copilot に命令ファイルを生成させることもできます。各環境での手順は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このワークショップでは、各環境で必要な手順を説明します。
>
> テンプレートや出発点を探す場合は、命令ファイル、カスタム エージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

変更による効果を確認するため、まずスターター プロジェクトに含まれる命令ファイルを確認します。

1. Codespace に戻ります。
2. Codespaces エディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの説明とコーディング ガイダンスを確認します。この命令は、リポジトリ内の Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro、Drizzle データ レイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、命令の適用対象を指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクト ファイルへのリンクを確認します。これらを使って、大きな命令セットを小さなファイルに分けて再利用できます。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ求めていません。次の手順で TSDoc コメントとファイル ヘッダーのルールを追加します。

## 変更用のブランチを作成する

ここではベスト プラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。演習によってモードとモデルの選択が異なるため、必要に応じて手順で指定します。

カスタム命令の効果を確認するため、まず既存の命令ファイルを使ってプロンプトを送信します。その後、命令を更新して、同じプロンプトを再送信します。

1. 前の演習で開いたエディター タブを閉じ、Copilot に必要なコンテキストだけを渡します。
2. Copilot が配置先を把握できるよう、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。Agent はファイルの変更を適用できます。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパー ファイルやテストなどを更新します。
6. 提案されたヘルパーは、最初の引数に `db` クライアントを受け取り、型付けされた発行者の配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に従っています。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、命令がなくてもドキュメント コメントを追加することがあります。その場合も問題ありません。命令を更新した後の一貫性の改善を確認するのが目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、プロジェクト全体の情報を Copilot に伝えるためのファイルです。コード提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加してファイル コメント ヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したとおり、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスク向けのガイダンスには `*.instructions.md` を使います。ドキュメント コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再度選択し、Copilot のコンテキストに含めます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルが次のようなコメント ブロックで始まることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しい標準に従うよう指示し、次の演習で使う実際のコードに適用しました。

## 命令とブランチをコミットしてプッシュする

命令ファイルもほかのアセットと同様に、ソース管理で管理します。変更をリポジトリにコミットしてプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、命令ファイルとヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで命令ファイルと、ヘルパーの基盤作成で Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、命令標準の Issue に関連付けてプルリクエストを作成します。手動で行うこともできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼します。Issue を見つけて PR を作成し、マージするように依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクト テンプレートには GitHub ツールがすでに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従ってください。この接続方法 (Model Context Protocol) は後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選ばれていることを確認します。
3. 命令ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。命令の更新と、次の絞り込み作業に備えた publishers ヘルパーの基盤の両方を説明に含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に GitHub 操作の許可を求められたら、コマンドを確認して適切なものを許可します。
6. PR が作成されたら、次のプロンプトで PR をマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作やシェル コマンドの許可を求められたら、コマンドを確認して適切なものを許可します。

これで GitHub Copilot の支援を受けてプルリクエストを作成し、マージできました。

## まとめと次のステップ

Copilot が命令ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の命令を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、命令とヘルパーの基盤を `main` にコミットしました。

次は、エージェント モードでこれらの命令を活用し、コードベース全体にわたる[新機能を Copilot に追加させます][next-lesson]。

## リソース

- [GitHub Copilot のカスタム命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
