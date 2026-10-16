---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

[**GitHub Copilot app**](https://docs.github.com/copilot/concepts/agents/github-copilot-app) は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を単一の作業用ワークスペースで実現します。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub Issue と pull request のネイティブ管理機能を備えています。さらに、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまで pull request を導く **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys の一連のワークフローに取り組みます。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースとバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーでレビューし、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター機能の Issue から開始し、**Plan** モードでアプローチを決め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの命令を更新して、フィルター機能に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) 用のカスタム エージェントを作成し、要件、テスト範囲、検証の証拠を確認します。
8. フィルター機能全体をレビューし、2 件目の PR に Agent Merge を使用します。
9. 既存の Database Explorer キャンバスを使用してから、リポジトリに保存するトリアージ キャンバスを作成してテストします。

ワークショップの焦点を絞るため、作成する PR は 2 件です。星評価と、命令、スキル、QA プロファイル、テストの更新を含むフィルター機能です。どちらも更新済みの `main` から開始します。フィルター機能と品質に関するワークフローでは、同じセッション、worktree、ブランチを使用します。最後のキャンバス演習はそのセッション内で行い、PR ワークフローを繰り返さず、共有領域の作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価を追加する: すばやくできる変更][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | Issue から機能を計画し、Autopilot で構築して Interactive モードでレビューする |
| [4. カスタム命令で Copilot をガイドする][ex4] | コンテキスト | 命令を確認して更新し、フィルター機能に適用する |
| [5. quality-checks スキルをカスタマイズして使用する][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を構成し、フィルターの動作を確認する |
| [7. QA エージェントを作成して使用する][ex7] | 要件とテスト範囲 | 専門プロファイルを作成して選択し、最終的な検証結果をまとめる |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター、命令、スキル、QA プロファイル、テストを確認して、2 件目の PR に Agent Merge を使う |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージ キャンバスを作成してテストする |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加リソースを振り返る |

## 前提条件

このワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランが設定された GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランを利用していない場合、認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の各機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は codespace ではなく自分のコンピューターで実行するため、[レッスン 0][ex0] では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、アプリを使用するには管理者が **Copilot CLI** ポリシーを有効にする必要があります。

## はじめる

[**レッスン 0「前提条件」から始める →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students