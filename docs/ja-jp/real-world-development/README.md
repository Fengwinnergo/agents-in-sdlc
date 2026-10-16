---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を段階的に計画してコードを作成し、動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

あなたは、開発者向けのテーマを取り入れたボードゲームのクラウドファンディングを運営する架空の会社、Tailspin Toys に入社したばかりの開発者です。チームのバックログは GitHub Issue として用意されており、すぐに取り組めます。カテゴリやページネーションによる絞り込みなどの機能開発に加え、アクセシビリティやコーディング規約の改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。開発したい環境に合ったワークショップを選んでください。どのワークショップも個別のセットアップから始まり、Tailspin Toys の共通シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、カスタム命令、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実践的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業方法をガイドし、進捗を確認して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントをエディターから離れずに扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
