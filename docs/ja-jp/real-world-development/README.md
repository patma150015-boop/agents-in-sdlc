---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を反復的に行います。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys の新しい開発者として作業します。チームのバックログは GitHub Issue として登録済みです。フィルターやページネーションなどの機能に加え、アクセシビリティやコーディング標準の改善にも取り組みます。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業する環境で利用できます。自分に合った環境を選んでください。各ワークショップには独自のセットアップ手順があり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright Model Context Protocol (MCP)、Agent Merge、実践的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業を行い、GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を導き、進捗を確認し、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントをエディターから離れずに扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
