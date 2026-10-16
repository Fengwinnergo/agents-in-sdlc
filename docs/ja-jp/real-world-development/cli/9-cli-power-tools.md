---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキスト管理、モデル選択、セッション共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

一般的な CLI ツールと同じく、GitHub Copilot CLI にも操作に使える多くのスラッシュ コマンドがあります。コマンドを使うと、高度な機能、内部情報、追加の構成オプションにアクセスできます。ここまでは `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。このレッスンでは、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、クラウド エージェントへの委任に使う任意のコマンドを確認する。

## シナリオ

CLI の基本的なワークフローを完了したので、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模なタスクや複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュ コマンドで状態を確認したり、自分で圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が開いていない場合はリポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域にコンテキストがどう分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな変化が見られないことがあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。タイミングを選びたい場合は `/compact` を使います。関係のないタスクに切り替えるために新しい会話を始める場合は、`/clear` または `/new` を使ってください。

## モデルを選択する

モデルにはそれぞれ異なる長所があり、開発者によって好みも異なります。Copilot CLI では、モデルの一覧を表示して使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用を改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで `/share` が使えることを確認します。
2. セッションを共有する場合は、`/share file` によるローカルの Markdown エクスポートなど、目的に合った方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡大または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大規模なタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列に実行します。
- `/worktree` は、別のタスク用に独立した Git worktree を作成します。
- `/delegate` はタスクを Copilot cloud agent に渡します。エージェントは非同期で作業し、プルリクエストを開くことがあります。

これらのコマンドは worktree の作成やリモートでの作業を伴うことがあるため、任意です。試す前に、範囲を明確にした新しいタスクを始め、通常のワークフローと同じように結果をレビューしてください。非同期エージェントについて詳しく知りたい場合は、[クラウド エージェントのワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュ コマンドを使うと、設定を変更したり、セッションを共有したり、内部の処理を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` で利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウド エージェントへの任意の委任に使うコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュ コマンドがあります。次は、ここまで学んだ内容と、さらに学ぶためのリソースを[振り返ります][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
