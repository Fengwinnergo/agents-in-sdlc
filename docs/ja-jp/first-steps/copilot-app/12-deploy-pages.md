---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを構成し、プルリクエストをレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages で Space Quiz をデプロイして、誰でもアクセスできるようにします。Copilot はワークフローの準備とデプロイを支援しますが、変更をレビューしてサイトが公開されたことを確認するのはユーザーの役割です。

このレッスンでは、次のことを行います。

- Copilot に GitHub Pages のデプロイを設定させる。
- マージ前にワークフローとプルリクエストをレビューする。
- デプロイを確認し、公開されたサイトを検証する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、ほかの人が試すための簡単な方法がありません。GitHub Pages を使って 1 つのファイルで構成されたサイトを公開すると、サーバーやランタイム依存関係を追加せずに公開 URL から利用できるようになります。

## Copilot にサイトのデプロイを依頼する

Copilot app で `space-quiz` のセッションを開き、次のプロンプトを送信します。

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Space Quiz のプルリクエストのマージ、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions のデプロイ、yourname.github.io/space-quiz/ に公開された Space Quiz の 3 段階を示す図。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> このプロンプトでは、エージェントにプルリクエストの作成とマージ、およびリポジトリの Pages 設定の変更を依頼します。実行する前に、差分、デプロイ権限、GitHub の承認要求を確認してください。

## ワークフローをレビューしてデプロイを確認する

1. リポジトリに README がまだない場合に限り、README が追加されていることを確認します。
2. ワークフローのビルドとデプロイの手順を含め、プルリクエストの変更をレビューします。
3. 必須のプルリクエスト チェックが成功した後にのみマージします。
4. GitHub Actions のデプロイ実行が成功するまで確認します。
5. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効にしたり、プルリクエストをマージしたりできない場合は、エージェントから示された指示に従います。デプロイが成功し、公開サイトが読み込まれるまでは、タスクを完了と見なさないでください。

## まとめと次のステップ

Space Quiz を公開し、エージェントの報告だけに頼らずデプロイを確認しました。[レッスン 13: レビューと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
