---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒なタスクを任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようにします。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確に定めた Issue の重要性を学ぶ。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には対応したい技術的負債があります。最初のサイトを作った請負業者は、ドキュメントを整備していません。実際、ドキュメントがまったくない状態です。最初の対応として、アプリケーションのエクスポート関数すべてに TSDoc コメントを追加します。

また、デザイン チームはゲームを探しやすくしたいと考えています。ゲーム詳細ページに、同じカテゴリのほかのタイトルを関連ゲームとして表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。現時点ではブロッカーですが、優先順位がより高い Issue がほかにあります。

このようなタスクは後回しになりがちです。Copilot cloud agent に割り当てると、開発者が別の作業を進めている間に非同期で処理させ、完了後に戻って結果を確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するためのドラフト プルリクエストを作成し、環境を準備してタスクに取りかかります。作業中または完了後に Copilot のセッションを確認できます。解決案のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見単純な操作でも、詳しく見ると複雑な場合があります。[cloud agent に依頼するタスクの範囲を明確に定めてください][cloud-agent-best-practices]。AI ペア プログラマーのように、段階的に作業し、学び、試し、状況に合わせて調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

これまでの演習で、カスタム命令とその効果を確認しました。すでにカスタム命令の演習を終えている場合は `.github/copilot-instructions.md` の動作を確認できているはずです。まだの場合は、ここで簡単に確認してください。cloud agent に作業を割り当てる前に、リポジトリに含まれている命令ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの GitHub Web UI、または起動中の Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この命令は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てます。作成された PR に TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されていることを確認してください。これらの内容は命令ファイルから適用されます。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

開発者が誰であっても、コード作成には通常、特定の環境と、正常な状態を整えるセットアップ スクリプトが必要です。ソフトウェア エンジニアと同様に作業する Copilot にタスクを割り当てる場合も同じです。

Cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。作業開始前に実行する[専用のセットアップ ワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、必要な開発ツールや依存関係を利用できるようにします。学習に集中できるよう、このラボ用の設定は事前に用意されています。Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、単一 Astro アプリ用に移行とシードを済ませた SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同様ですが、いくつかの重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブから手動でワークフローを実行できます。Copilot が実行するまで待たずに、セットアップが成功するか確認できます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古かったり、まったく存在しなかったりするプロジェクトが多くあります。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成が得意なので、必要なドキュメントを生成するタスクを cloud agent に依頼できます。後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの、新しく生成されたプルリクエスト (PR) を開きます。すぐに表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が PR に todo リストを作成します。

> [!NOTE]
> Copilot が todo リストを表示するまでに数分かかる場合があります。Copilot が環境を準備し (前述のワークフローを実行し)、プロジェクトを分析して、最適な方法を決めているためです。

13. todo リストと、Copilot が実行しようとしているタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するために、ブラウザーを更新する必要がある場合があります。

16. ライブ セッションをスクロールして、Copilot の作業を確認します。コードと状態の調査、計画を考えて適切な方法を決める過程、コードの作成などが表示されます。

作業には数分かかるでしょう。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を利用して、別のタスクも cloud agent に割り当ててから、アプリケーションに機能を追加する作業に進みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザイン チームのために関連ゲームのセクションを追加する作業に時間がかからないとしても、その時間をほかのタスクに使えます。関連ゲーム機能を cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. 空のテンプレートを使用するには **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取りかかっています。Copilot cloud agent はソフトウェア エンジニアと同様に作業するため、常に監視する必要はありません。完了してからレビューしてください。次はカスタム エージェントの作成と利用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を確認しました。cloud agent に Issue を割り当てると、非同期で作業できます。技術的負債への対応、新機能の作成、別のフレームワークからのコード移行などを依頼できます。

次の概念について確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に実行すること。
- 範囲を明確に定めた Issue の重要性。
- Issue を Copilot に割り当てること。

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と利用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム命令も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
