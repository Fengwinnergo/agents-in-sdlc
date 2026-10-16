---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使用する"
description: "既存の quality-checks スキルを確認してレポート形式をカスタマイズし、絞り込み機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、コードの検証が必要です。これまでは手動で動作を確認し、命令ファイルを使って標準に沿うようにしてきました。テスト、lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうしたタスクには、**エージェント スキル**が適しています。スキルを使うと、これらの作業を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、`quality-checks` スキルを使って単体テスト、lint、型チェックを実行します。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの命令、実行可能なスクリプト、補助リソースがまとめられています。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` ファイルで構成されます。Markdown ファイルには、スキル名と説明を定義するフロントマター、処理の概要、呼び出すタイミングのガイダンスを記述します。スキルが使用するスクリプトやその他のリソースをサブフォルダーに置くこともできます。

> [!NOTE]
> スキルに追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルが含むファイルは `SKILL.md` だけです。プロジェクトに既存のコマンドを使用するためです。

スキルは、チームで共有して再利用するプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーのスキル フォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に用意した `quality-checks` スキルを見てみましょう。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。`description` は、Copilot がスキルを呼び出すタイミングを判断するために使います。
3. 命令を読んで、テストと lint の手順がどのように示されているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

Copilot CLI で直接スキルを呼び出すことも、自然言語で依頼することもできます。Copilot に 3 種類のチェックを実行してもらいます。

1. Interactive モードで絞り込みの会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

何を実行したか、成功したか、ツールが実際に何を報告したかがわかるレポートに改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されます。

## 更新したスキルを実行する

変更後のスキルを実行します。Copilot CLI では、会話を再起動しなくても編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. レポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は絞り込み機能の PR に含めます。次は、[Playwright MCP サーバーを使って Copilot にサイトを操作させます][next-lesson]。

## スキルの例

コミュニティの次の例は参考資料です。この演習で追加するタスクではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
