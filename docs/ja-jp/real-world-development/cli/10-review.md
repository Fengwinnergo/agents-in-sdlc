---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 件のプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローを進めました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、事前に用意された絞り込みの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込みの Issue から始め、Plan モードで方法を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) 用のカスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を確認しました。
- 絞り込み機能の変更全体をレビューし、絞り込みの PR に Agent Merge を使用しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュ コマンドを確認しました。

## 作成したもの

このワークショップには、更新した `main` からそれぞれのブランチを作成して進める、2 つの PR があります。

1. **星評価**: 既存の `starRating` と、評価がない場合に表示するメッセージをゲーム カードに追加しました。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、命令を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルと関連テストを追加しました。

ワークショップを効率よく進めるため、絞り込みを計画してから PR を作成するまで、同じ会話とブランチを使いました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー確認という複数の方法でコードを検証しました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、指定した形式で結果を報告しました。QA は、要件とテスト範囲のレビューにその結果を加えて確認しました。

追加するテストは、実際に不足している確認を補うものである必要があります。新しいテストが不要という QA の結果が妥当な場合もあります。マージを承認する前に、コードと検証の証拠を確認してください。変更を加えたら、影響を受ける証拠も更新します。

## ベスト プラクティス

Copilot に渡すコンテキストとツールが、その動作に影響します。このワークショップでは、命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方法を検討するには **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

ツールと同様にコンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力は大きく変わります。

## さらに試す

ワークフローの基本を学びました。次の CLI 機能も確認してみてください。

- `/review` でコード レビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を説明し、別の観点からの意見を得ます。
- `/fleet` で独立したサブタスクを並行して実行します。
- `/worktree` で別のタスクを独立させます。
- `/delegate` でタスクを Copilot cloud agent に渡します。

## 次のステップ

ツールの使い方を上達させるには、実際に使い続けるのが一番です。本番コードや趣味のコード、長年作ろうと思っていた小さなアプリなどで試してみましょう。チームで学んだことを共有し、ほかのメンバーからも学んでください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。ほかの環境での体験と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
