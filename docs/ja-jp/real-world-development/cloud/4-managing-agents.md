---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、GitHub Copilot cloud agent にユーザー体験を改善して機能を追加する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールはいくつもあります。この mission control では、オープン中のプルリクエスト (PR) を持つすべてのエージェント タスクを確認できます。実行された操作を調べたり、進行中のセッションに指示を追加したりできます。

このレッスンでは、次のことを行います。

- cloud agent のタスクを監視するために agents ページを確認する。
- 進行中のセッションに追加機能を依頼し、方向付ける。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加することにしました。サイトのスタイルを更新して切り替え機能を追加する作業が進んでいるため、同じ作業に含めるのが合理的です。エージェントにライト モードも追加するよう指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインの両方にあるタスク一覧を確認します。割り当てたタスクとして、次の項目が表示されるはずです。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。次のような操作があります。
    - リポジトリをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するためのさまざまな手順を行う。

> [!NOTE]
> 表示される手順は、Copilot の作業状況とアプローチによって異なります。

4. 右側に表示される PR ペインも確認します。PR と変更ファイルを確認して、追加で監視できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認したので、ハイ コントラスト モードの作業中にライト モードの切り替えも追加するよう Copilot に依頼します。

1. ハイ コントラスト モードの追加に関するセッションを選択します。表示されるタイトルは、Copilot が使用する名前と作業の状態によって異なります。

    ![mission control のアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページで、作業中のセッションに指示するダイアログが強調表示された cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に含めることを確認します。

## Copilot に作業を続けてもらう

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順を終えてから、新しい依頼を作業に反映します。

今回も数分かかるので、これまで学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視してガイドする中心的な場所である、Copilot agents ページを確認しました。この mission control では、進行中および完了したすべてのタスクを追跡し、実行された作業を確認して、進行中のタスクの範囲を変更したり、追加のガイダンスを与えたりできます。

このレッスンでは、次のことを行いました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
