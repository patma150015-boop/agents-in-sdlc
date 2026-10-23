---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書く以外にも、データや外部サービスを扱い、Copilot が利用できる自動化機能を追加する必要があります。ここで役立つのが MCP サーバーです。MCP サーバーを使うと、CLI に組み込まれた機能を超えて、Copilot にツールやサービスを提供できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を確認します。
- まだ利用できない場合は、Playwright MCP サーバーを追加します。
- エージェントにブラウザーを操作させ、フィルター機能を試します。

## シナリオ

単体テストとエンドツーエンドテストは重要ですが、ユーザーインターフェイス (UI) の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように Web サイトを使えるようにして、変更が期待どおりに動作する確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスと通信できます。AI エージェントは MCP を介して外部ツールやサービスとリアルタイムに通信し、最新情報を取得したり、開発者に代わって操作したりできます。

ツールやリソースへのアクセスには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを使います。各 MCP サーバーは、AI エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を利用できます。

ほかにもさまざまな MCP サーバーがあります。GitHub は、エコシステム内での発見と貢献を促す [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトのほかの依存関係と同じように扱ってください。使用する前にソースコードを確認し、発行元を検証して、セキュリティへの影響を考慮してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Playwright MCP がすでに Copilot CLI に設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力してから、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のコマンドを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で Copilot に機能を試させる

先ほどは、機能が期待どおりに動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. Copilot に Playwright MCP サーバーを使って検証するよう依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常、Copilot は MCP サーバーを自動で判断するため、使用を指示する必要はありません。ただし、使うべきツールが分かっている場合は明示すると、結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を検証するために実行する手順を確認します。
3. レポートを読み、すべてが期待どおりに動作することを確かめます。

Copilot はサーバーを起動し、Playwright で Web サイトを操作してからサーバーを停止し、結果を報告します。

## まとめと次のステップ

Copilot CLI から実際のブラウザーで機能を試すため、Playwright MCP サーバーを使いました。具体的には、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使われ方を確認しました。
- まだ利用できない場合は Playwright MCP サーバーを追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を試しました。

次は、スキルとブラウザーツールを専門的な役割にまとめる[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
