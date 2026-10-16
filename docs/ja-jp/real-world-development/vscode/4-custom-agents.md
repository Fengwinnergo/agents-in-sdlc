---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーにある Markdown ファイルでエージェントを定義し、特定の作業をより効果的に行うための命令、ベスト プラクティス、コーディング パターン、専門知識を Copilot に伝えます。チームの知識を再利用可能なエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、一貫したテスト パターンを維持するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、または全体に適用される `~/.copilot/agents` 内の Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタム エージェントとエージェント スキルの比較

カスタム エージェントと[エージェント スキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業の進め方を伝えます。最も簡単な区別は、**カスタム エージェントは作業者、スキルはツール**という考え方です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを調整します。このラボでは、アクセシビリティ用カスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエスト ワークフローやテストの実行・管理のためのスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法は 1 つではありません。AI を使ったほかの作業と同様に、環境やシナリオに合わせてテストと反復を行ってください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべてのユーザーがクラウドファンディングのプラットフォームを利用できるようにしたいと考えています。ユーザーから、現在のダーク テーマはテキストと背景色のコントラストが不十分で読みづらいというフィードバックがありました。このアクセシビリティ上の問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装する必要があります。カスタム エージェントを使って機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ用カスタム エージェントを確認する。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイ コントラスト モードを実装する。

## アクセシビリティ用カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに作成されています。内容を確認し、Copilot をどのようにガイドするかを把握します。

Codespace に戻り、アクセシビリティ作業用に `main` から新しいブランチを作成します (Exercise 3 の絞り込み PR と分けるためです)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタム エージェントに必須です。

3. 続くセクションを確認します。アクセシブルな Web サイト用コードを生成するときの主要な責務、ベスト プラクティス、HTML、CSS、JavaScript のコード例、よくある落とし穴や誤りが記載されています。

## Copilot Chat でカスタム エージェントを使う

VS Code では `.github/agents` に定義されているすべてのカスタム エージェントが、Copilot Chat ビュー下部のエージェント ドロップダウンに表示されます。カスタム エージェントを選ぶと、そのエージェントの命令とツールに合わせてチャット セッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なるため、必要に応じて手順で指定します。

1. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部のエージェント ドロップダウンを選択します。既定では、現在のエージェント (**default**) が表示されています。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルに適用します。各編集はファイル パスと差分とともにチャットに表示されます。
6. エージェントが変更したファイルに応じて、**Files changed** の表示が更新されます。

> [!NOTE]
> Copilot はコードベースを調べ、Astro コンポーネント、CSS、関連テストなどを変更するため、数分かかる場合があります。

次の演習では、作業中の変更をレビューして方向付けます。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]について学びました。カスタム エージェントは、特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識と標準を再利用可能なエージェントにまとめ、Copilot が特定の作業をより効果的に行えるようにできます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Copilot Chat の Agent Mode でカスタム エージェントを使いました。

次は、[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認し、同じセッションにライト モードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
