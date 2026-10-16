---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードで絞り込み機能を作成しました。プルリクエストを作成する前に、ブラウザーで正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する方法を学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に MCP サーバーを使わせ、ブラウザーで絞り込み機能を手動テストする。
- ブランチを公開して絞り込み作業のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

エディターの外部にもアクセスできるようになると、エージェント モードはさらに強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準です。

![コンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) と Agent Mode の連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信できるようにします。MCP を使うと、AI エージェントはリアルタイムに情報へアクセスし、ユーザーに代わってアクションを実行できます。

ツールとリソースには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージのようなローカル ツールなど、外部ツールとの通信を管理します。それぞれの MCP サーバーが、AI エージェントから利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などが可能です。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などが可能です。

ほかにも多くの MCP サーバーがあり、GitHub はエコシステムの発見と貢献を促進するために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同様に扱ってください。利用する前にソース コードを確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能のテストに利用します。

1. `.vscode/mcp.json` に `github` と並べて `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル** サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずに Playwright を実行します。画面を表示するデスクトップ環境がない Codespace では、このオプションが必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはすでにエンドツーエンド テストに Playwright を使用しているため、通常は必要なブラウザーもインストール済みです。ブラウザーがないと Copilot から報告された場合は、`npx playwright install chromium` を実行して再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がそのサーバーのツールを初めて必要としたときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか尋ねたら、構成を確認し、信頼して起動します。

手動で起動する必要はありません。スキップした場合、次の手順で Copilot がツールを必要としたときに VS Code がサーバーを起動し、信頼の確認を求めます。

## 絞り込み機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントにページを開かせ、移動やフィルターの適用、結果の確認をさせて、その内容を要約させます。会話を離れずに機能の動作をすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使います。そのため、ボタン、リンク、リスト項目など、ラベル付けされた構造化要素をエージェントが確認します。支援技術と同じ方法で情報を扱うため、機能確認と同時に簡単なアクセシビリティ チェックもできます。

サーバーを接続し、アプリを実行した状態で、Copilot に次のプロンプトを送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。その要約を Issue の受け入れ条件と照らし合わせます。問題があれば、PR の前に追加質問をしたりコードの修正を依頼したりします。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリが実行されている必要があります。開発サーバーを停止した場合は、プロンプトの前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードすることがあります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace で **Source Control** パネルを開き、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![Source Control パネルに Copilot が行った変更が表示されている画面](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) など、いくつかの方法でプルリクエストを作成できます。ここでは GitHub Copilot に作成させます。関連する Issue を探し、それに関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。これにより Copilot は GitHub ツールを使えます。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Copilot が情報を収集して操作するために求められたら **Continue** を選択します。GitHub ツールを初めて使うときに認証を求められたら、画面の指示に従います。
5. Copilot が Issue を検索し、適切なものを見つけて PR を作成します。
6. Copilot が表示するリンクを選択して PR をレビューします。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーで絞り込み機能を手動テストしました。
- ブランチを公開し、絞り込み機能のプルリクエストを作成しました。

MCP サーバーで機能をテストしましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューのような特定のタスクを効率化する[カスタム エージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
