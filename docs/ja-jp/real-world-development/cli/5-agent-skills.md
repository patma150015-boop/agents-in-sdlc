---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、開発にはさまざまな作業があります。手動でコードを検証し、指示ファイルを使って標準に従わせました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には**エージェントスキル**が適しています。スキルを使うと、これらの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースが含まれ、エージェントが必要に応じて読み込みます。スキルは通常、スキル名のフォルダーと、その中にある `SKILL.md` で構成されます。Markdown ファイルには、スキル名と説明を定義するフロントマター、概要、スキルを使用するタイミングのガイダンスを記載します。必要に応じて、スクリプトなどのリソースをサブフォルダーに追加できます。

> [!NOTE]
> スキルに追加のファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームと共有して再利用するためにプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーのスキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys チームが単体テスト、lint、型チェックの実行に使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 冒頭の `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、テストと lint のプロセスを導く方法を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルをカスタマイズする前に、Copilot に3つのチェックを実行させます。

1. Interactive モードでフィルター機能の会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 実行後のレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールの出力が分かるレポートに改善します。結果の内容を指定するセクションをスキルに追加します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

変更したスキルを実行して、新しい形式が反映されることを確認します。Copilot CLI では、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 変更前と同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- 単体テスト、lint、型チェックを実行する `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルター機能とともに機能の PR に含まれます。次は、[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## その他のスキル例

以下はコミュニティの参考例であり、追加の作業ではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と付属のエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
