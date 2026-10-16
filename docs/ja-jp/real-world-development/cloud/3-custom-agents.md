---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより効果的に行うための命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー内の Markdown ファイル、または全体に適用される `~/.copilot/agents` 内のファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**という考え方です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整します。この演習では、アクセシビリティ エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、必要に応じて更新します。その作業では、プルリクエスト ワークフローのスキルやテストの実行・管理用スキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法は 1 つではありません。AI を使ったほかの作業と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、次のことを確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングのプラットフォームを利用できるようにしたいと考えています。ユーザーから、現在のダーク テーマはテキストと背景色のコントラストが不十分で読みづらいというフィードバックがありました。このアクセシビリティ上の問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認し、Copilot をどのようにガイドするかを把握します。

Codespace に戻り、アクセシビリティ エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを確認します。アクセシブルな Web サイト用コードを生成するときの主要な責務、ベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や誤りが記載されています。

## Issue を作成して割り当てる

Mission control は、環境内のエージェントを操作するための場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、進行中のセッションに追加の指示を出せます。まず、ハイ コントラスト モードの作成タスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![アクセシビリティ エージェントが選択された cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドでタスクを開始します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるようにできます。

この演習では、次のことを確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方。

Copilot がハイ コントラスト モードを実装している間に、次は mission control から[エージェント セッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [organization でカスタム エージェントを使う準備][org-custom-agents]
- [enterprise でカスタム エージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
