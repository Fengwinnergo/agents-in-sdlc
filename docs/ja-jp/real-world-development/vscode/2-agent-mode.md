---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、通常は複数のファイルを変更し、テストを実行するなどの作業が必要です。開発者は必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更漏れのファイルを見つけてさらに更新します。こうした作業は繰り返し発生します。

このようなときに Copilot Agent Mode が役立ちます。

Copilot Agent Mode は IDE でより自律的に動作します。開発者と同じように、まずプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、問題が見つかったら自動的に修正します。Agent Mode を使ってサイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、Agent mode と cloud agent は異なる体験を提供します。Agent mode は IDE 内で作業し、素早いフィードバックや対話を可能にします。Cloud agent はチーム メンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更を行う。
- UI とデータ レイヤーにまたがる新機能を実装する。
- Copilot が生成した変更とテストをレビューしてからコードベースにマージする。

## シナリオ

ゲームの一覧が増えてきたので、ユーザーがカテゴリと発行者で絞り込めるようにします。前の演習で publishers ヘルパーを追加しました。Copilot Agent Mode を使って、データ レイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握します。

この Web サイトは、開発者向けのテーマを取り入れたボードゲームのクラウドファンディング プラットフォームです。ゲームを一覧表示し、詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML としてページをレンダリングします。ページは frontmatter 内で Drizzle ORM を通じてローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータ アクセス ヘルパーは `src/lib/` にあり、インタラクティブな動作には標準 DOM API を使う、小さな範囲に限定した Astro `<script>` を追加します。

### Web サイトを起動する

単一 Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を行います。

1. 前の演習の `main` に戻っていることを確認します。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドで Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが準備できると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストする Codespace へのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行者、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要に応じて手順で指定します。

Web サイトの最初の実装は動作しますが、新しい機能を追加します。プロジェクトをテンプレートから作成したとき、Issue のバックログも自動的に用意されました。GitHub Copilot にバックログ項目を表示させます。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は自律的に計画を立て、複数ファイルの変更、ターミナル コマンドの実行、ツールの呼び出しを行います。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、快適に進められるよう手順に含めています。LLM は確率的に動作するため、受け取る提案が例と異なる場合があります。これは正常で、想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使うことがありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信し、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択し、すべての Issue を一覧表示するコマンドを実行します。
5. Copilot が作成した Issue の一覧と優先順位を確認します。

ユーザーに最も役立つと Copilot が判断した項目から順に並んでいることを確認します。

## 命令ファイルを確認する

Agent にコードを生成させる前に、作業のコンテキストとして使う命令ファイルを確認します。Web サイトの機能追加に関するガイダンスを含む [ユーザー インターフェイス (UI) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加に関する全般的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト可能性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類別の命令ファイルへのリンク。

> [!TIP]
> 命令ファイルから、ほかの命令ファイルとプロジェクト ファイルの両方を参照できます。パスは命令ファイルからの相対パスです。これを使って再利用しやすい小さなファイルに分けたり、Copilot に例やテンプレートを示したりできます。

## 絞り込み機能を実装する

絞り込み機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ レイヤー (`src/lib/`) の絞り込みロジックを追加または改善する。
- 絞り込み動作のテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功する必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して命令ファイルを追加します。

   ![UI 命令ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. 関連するバックログ Issue の要件に基づいて実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを特定する様子を確認します。データ レイヤー、UI、テストのファイルを見つけて変更し、テストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストなどを実行します。タスクやコードベースの複雑さによって、数分かかることがあります。作業中は、エディターに **Keep** と **Undo** が表示される場合があります。作業中に選択する必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。エラーを検出しなくなるまで、Copilot はコード生成とテスト実行を繰り返します。

   ![Copilot Agent Mode のセッション完了画面](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードをレビューすることは常に重要です。

10. Web サイトを実行中のブラウザーに戻り、新しい機能を試します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに新しい機能を追加する方法を確認しました。次のことを学びました。

- Copilot Agent Mode で UI とデータ レイヤーにまたがる新機能を実装できる。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにまたがる変更ができる。
- Copilot Agent Mode が生成した変更とテストをレビューしてから、コードベースにマージする。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### 追加の演習 — ページングを実装する

ゲーム一覧が増えると、ページングが必要になります。この演習で学んだことを使って、Copilot にページングの実装を依頼してみましょう。

- 既存の命令ファイルを使うなど、ベスト プラクティスに従います。
- ページ サイズをユーザーが選択できるようにするか、固定にするかなど、実装方法を決めます。
- 希望する実装に必要なガイダンスをプロンプトで Copilot に伝えます。
- Copilot に変更を依頼し、コンテキストを与えながら反復することがあります。これは Copilot を使った通常の作業です。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使いどころ][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
