---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で実装方法を決め、Autopilot で Issue に基づく絞り込みを構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初のレッスンでは、小さな機能をプロジェクトに追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は、組織がすでに採用している流れに沿って、必要な機能を適切な方法で構築できるよう支援します。このレッスンから複数のレッスンにわたって、Issue から機能を実装し、コードを検証して動作を確認し、最終的にプロジェクトへマージする、エージェントを活用した開発プロセスを進めます。

> [!NOTE]
> 機能開発のワークフローを続ける間、同じ会話とブランチを使います。通常は、扱うファイルの種類ごとにブランチや PR を分けますが、ここでは中心となる概念に集中できるよう簡略化しています。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot の会話を始める。
- Plan モードで要件を決める。
- Autopilot モードで新しい機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動検証する。

この機能開発を続けながら、リポジトリの命令を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや発行者でゲームを絞り込めるようにする必要があります。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、実装前に決めておきたい点があります。Plan モードで方針を決めてから、範囲を明確にした実装を Autopilot に依頼します。

## 背景

AI コーディング エージェントを開発フローに取り入れても、基本は変わりません。むしろ、その重要性は高まります。多くの開発者は、次のような流れで作業します。

1. 実装内容を記述した Issue から始める。
2. 実装する内容を計画する。
3. コードを作成してレビューする。
4. テストを実行してコードを検証する。
5. 新しい機能を手動で確認する。
6. プルリクエスト (PR) を作成する。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージする。

> [!NOTE]
> チームや組織によって細かな手順は異なりますが、一般的にはこの流れを応用できます。

この標準的な方法に従うことで、AI が生成したコードも、手作業で書いたコードと同じ要件や確認プロセスに沿って扱えます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を持たせるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと協力して作業します。エージェントは変更を提案し、次の操作の前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動してください。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新して、絞り込み作業用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` のような表示が追加されます。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## 絞り込み機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に依頼する前に、実装方法を決められます。複雑な作業では、事前に時間をかけて計画することをお勧めします。Plan モードに切り替えて計画を作成します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下のモード表示が **Plan** になっていることを確認します。
2. 先ほど追加した Issue への参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot がプロジェクトを調べて、最適な方法を検討します。

3. 絞り込みの動作について Copilot から質問されたら、自分の希望に合わせて回答します。唯一の正解はありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成して確認したので、実装に進みます。Autopilot を使って Copilot に作業を任せます。Autopilot を使うと、Copilot は問題が解決したと判断するまで反復できます。

1. **Accept plan and build on autopilot (recommended)** または、インストール済みのバージョンで同等のラベルが付いたオプションを選択します。
2. プロンプトの下に表示されるモードが **Autopilot** であることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係がない、またはポートが競合していると Copilot から報告された場合は、チェックを完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同様に、マージ前にレビューします。コードをレビューし、サイトを実行して正しく動作することを確認します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を入力し、絞り込み機能の実装とテストを確認します。
3. 結果を Issue と計画時に決めた内容と照らし合わせます。
4. コードのレビューが終わったら <kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を確認する

コードに問題がなくても、実際に動かしてみましょう。前回と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が使用可能になったら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリや発行者、およびそれらの組み合わせによる絞り込みを試します。
4. リセットと該当する結果がない場合の動作が、Issue と決定した内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて機能を構築し、レビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで新しい機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、カスタム命令を使ってコード生成を文書化された手法に沿わせる方法を[確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
