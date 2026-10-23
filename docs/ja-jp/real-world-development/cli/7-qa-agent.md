---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルター機能を確認しました。次は、明確な品質保証 (QA) プロセスを定めたカスタムエージェントに、これらの機能をまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールを使う方法を確認します。
- 再利用可能な QA プロファイルを作成して確認します。
- QA エージェントを選択し、フィルター機能の Issue と照らし合わせて結果を確認します。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な構成です。プロファイルにはエージェントの目的、指示、利用可能なツールを記載します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し可能なチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、結果を報告するよう Copilot に指示します。プロファイルは各機能の代わりになるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、Copilot に再利用可能な QA プロファイルを作らせます。プロファイルには、QA が実行するチェックと守るべき境界を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、カスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、Copilot が意図した QA の流れと境界を設定したか確認します。これにより、検証だけを行いたいときに不完全または範囲が広すぎるエージェントが機能を変更するのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は任意ですが、含めるとエージェントの表示名が分かりやすくなります。
3. プロファイルの指示を読み、QA が要件を起点にし、リポジトリの指示に従い、`quality-checks` スキルと Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードの変更前に確認を求め、変更をコミットしたり PR を作成したりしないことを確認します。
5. いずれかの責務や境界が不足していたら、続行する前に既定のエージェントへ修正を依頼します。

## Issue を QA で確認する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルター機能の会話を再開し、QA を選択すると、Issue と計画時の決定事項を利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 作成したばかりのエージェントは、使用可能なエージェントの一覧に表示されない場合があります。その場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトで QA エージェントに機能のレビューを依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 作業の完了後、レポートを読みます。

## まとめと次のステップ

再利用可能な専門的役割をワークフローに追加し、結果を確認しました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールを使う方法を確認しました。
- 再利用可能な QA プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルター機能の Issue と照らし合わせて結果を確認しました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートがレビューできる状態になりました。次は、[機能 PR にまとめて Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
