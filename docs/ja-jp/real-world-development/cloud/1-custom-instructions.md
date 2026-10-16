---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクに特定の手順が必要な場合や、Copilot が知っておくべき背景情報がある場合は、その情報にアクセスできるようにします。[命令ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム命令とパス別の命令ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わるしくみを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のワークショップと異なり、ここでは命令を追加する前後で同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムに前後を比較するのは困難です。このワークショップの後半で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認できます。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発の手順に関するガイドラインと要件があります。

- データ レイヤーには常に単体テストが必要です。
- UI はダーク モードにし、現代的な印象にします。
- コードのドキュメントには TSDoc コメントを使用します。
- 各ファイルの先頭に、そのファイルの機能を説明するコメント ブロックを記載します。

命令ファイルを使って、Copilot がこれらの実践に沿ってタスクを実行できるようにします。

### カスタム命令

カスタム命令を使うと、コンテキストや好みを Copilot に伝え、コーディング スタイルや要件を理解しやすくできます。関連性の高い提案やコード スニペットを生成するための強力な機能です。コーディング規約、ライブラリ、コメントの形式などを指定できます。リポジトリ全体に適用する命令と、特定の種類のファイルに適用する命令を作成できます。

命令ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信されます。Copilot へのチャットや CLI のリクエストの多くに関係するプロジェクト全体の情報 (技術スタック、プロジェクトの概要、ベスト プラクティス、共通のガイダンスなど) を記載します。
- `.github/instructions/*.instructions.md` ファイルには、特定のタスクやファイルの種類に適用するガイドラインを記載します。TypeScript や Astro などの言語、UI コンポーネントの作成、単体テストの作成などに使用できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成に使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの命令と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の命令ファイルも[サポートしています][custom-instructions-support]。

### 命令ファイル管理のベスト プラクティス

命令ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトに代表的な方法が示されています。

- `copilot-instructions.md` には、作成するもの、プロジェクトの構造、全体的なコーディング標準など、プロジェクト全体に関わる内容を記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ レイヤーなどのファイル種別や、特定のタスクに関する命令を記載します。
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

このリポジトリには、コアの `copilot-instructions.md` と、複数のタスク向けの `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** に関する入れ子のガイダンスにも注目してください。これらは Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro、Drizzle データ レイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。このフィールドは、リポジトリのルートからの glob で適用対象を指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象です。
5. このプロジェクトの単体テスト作成に関するガイダンスを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや既存のプロジェクト ファイルへのリンクがあります。大きな命令を再利用可能な小さなファイルに分割し、コード生成の例を Copilot に示せます (パスはリポジトリのルートではなく、この命令ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはコーディング標準が記載されていますが、コード内ドキュメントはまだ求めていません。次の手順で TSDoc コメントとファイル ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で行う編集は、TSDoc コメントとファイル レベルのコメント ヘッダーをコードに追加するというプロジェクト全体のルールを設定することです。cloud agent は、このワークショップの後半で割り当てる Issue に取り組むときにこのルールを使用します。

編集の前に作業用ブランチを作成します (cloud agent は Issue の対象ブランチから命令を読み込みますが、ここでは変更を整理してコミットします)。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は、プロジェクト全体の情報を Copilot に伝えるためのファイルです。コード生成の改善に向けて、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内ドキュメントのルールがないため、生成されたヘルパーにドキュメント コメントがありません。
3. 既存の標準の直後に、次の Markdown を追加し、ファイル ヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、リポジトリ全体のガイダンスには `.github/copilot-instructions.md` を、特定の言語、ファイルの種類、タスク向けのガイダンスには `*.instructions.md` を使います。今回追加したドキュメント コメントのルールなど、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## 命令の更新をコミットしてプッシュし、マージする

Cloud agent は、Issue の対象ブランチにある命令ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチが作成されます。そのため、cloud agent が命令を使えるようにするには、`main` に変更をマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの命令を読み込みます。

> [!TIP]
> ワークショップで `main` に直接作業する場合は、ブランチを作成せずに変更を `main` にコミットしてもかまいません。ここでは、実際のプロジェクトの作業方法に近づけるため、ブランチを使っています。

## まとめと次のステップ

命令ファイルを通じて Copilot がコンテキストを得る方法を確認し、リポジトリ全体に適用する標準を追加しました。この標準は、以降の演習で cloud agent が作成するプルリクエストに適用されます。

次は、最初の Issue を [Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot の命令ファイル][instruction-files]
- [カスタム命令を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム命令を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 命令ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
