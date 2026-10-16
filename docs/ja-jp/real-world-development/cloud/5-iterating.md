---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的として、GitHub Copilot にいくつかのタスクを依頼しました。コードのドキュメント作成、デザイン チームが試せる関連ゲーム機能の実装、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善します。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チーム全体でレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期かつ無人でタスクを実行するため、安全性を確保するセキュリティ制約があります。

- Copilot はリポジトリへの読み取りアクセスを持ち、書き込みできるのはコード用に使用するブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で実行され、独立した一時的な作業環境を作成します。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が最初に作成した、コードへのドキュメント追加 PR を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。Copilot は、外部 MCP サーバーへの接続を含め、既定で外部リソースへのアクセスが制限されているため、これは**想定された動作**です。必要に応じて、[Copilot cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR 一覧を開きます。
3. **Add missing documentation** などのタイトルが付いたプルリクエスト (PR) を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。その場合は完了まで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択して変更を確認します。

    ![変更されたファイルのタブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc コメントなどのドキュメントを確認します。実際の変更は環境によって異なります。Exercise 2 の最初に確認した命令ファイルから、TSDoc コメント、TypeScript の規約、コメント ヘッダーが反映されていることに注目します。
6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認を待っていることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。正常に進むと、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に異なるため、依頼する内容を厳密には指定できません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、そのファイルの簡単な説明をコメント ヘッダーとして追加する。
- TypeScript と Astro ファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. PR またはコードのコメントで **@copilot** をメンションして、生成されたドキュメントへの変更を依頼します。上記の例、またはコードベースのドキュメントに関する別の提案を使います。
2. **View Session** を選択して、Copilot の作業を確認します。Copilot が更新用に新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復できます。準備ができたら、PR をドラフトからレビュー可能な状態に変更して、`main` ブランチにマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対して、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page**、または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの実行承認を待っていることを示すインジケーターを確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに追加されます。正常に進むと、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。cloud agent と反復して更新を依頼できます。たとえば表示する関連ゲームの数を変更したり、コメント ヘッダーと TSDoc コメントを追加したりできます (このタスクはカスタム命令を更新する**前**に割り当てました)。前と同じように、**Conversation** タブで新しいコメントを追加します。Copilot がコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタム エージェントが実装した機能を確認します。この PR には Exercise 3 で依頼したハイ コントラスト モードと、Exercise 4 の mission control から依頼したライト モードが両方含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website**、または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。数分かかることがあります。作業が完了するまで待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の利用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認を待っていることを示すインジケーターを確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。正常に進むと、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイ コントラスト モードとライト モードの切り替えを確認します。ページの再読み込み後も設定が保存されていることを確認してください。

アクセシビリティ用のカスタム エージェントが、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしたことを確認してください。気になる点や改善点があれば、前の PR と同じようにコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて、別の作業に集中するときに最も役立ちます。習慣を身に付けるため、リポジトリにいくつか Issue を登録し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ワークショップを完了しました。このレッスンでは、次の作業を行いました。

- **このリポジトリに用意されているカスタム命令ファイルを確認し、cloud agent の出力への影響を確認しました。**
- **Issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。**
- **カスタム エージェントを作成して使用し、ハイ コントラストとライト モードを追加しました。**
- **Copilot agents ページを mission control として使い、アクセシビリティのセッションを監視して方向付けました。**
- **Copilot のプルリクエストをレビューして反復し、`@copilot` に変更を依頼し、ワークフローを承認しました。**

## レビューと次のステップ

Cloud agent ワークショップが完了しました。さらに学ぶ場合は、ほかのワークショップで異なる環境のシナリオを試してください。

- 🖥️ **[VS Code ワークショップ](../../vscode/)** — IDE で Copilot Chat のエージェント モードと MCP 統合を体験します。
- 💻 **[CLI ワークショップ](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、cloud agent への任意の `/delegate` を使って、レビュー済みの変更を 2 件ターミナルからリリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野に特化したカスタム エージェントを作成する。
- 別の技術スタック用に `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
