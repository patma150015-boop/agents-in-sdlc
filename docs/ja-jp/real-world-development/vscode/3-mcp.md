---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルター機能を構築しました。プルリクエストを作成する前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を確認します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせ、ブラウザーでフィルター機能を手動でテストします。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできるとさらに便利です。Copilot が外部にアクセスするための仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準規格です。

![コンテキスト、LLM、ツール、および MCP サーバーや VS Code 拡張機能が提供するツールとエージェントモードの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を使うと、AI エージェントはリアルタイムで外部のツールやサービスと通信できます。リソースから最新の情報を取得し、ツールを使って開発者に代わって操作できます。

ツールやリソースへのアクセスには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを使います。MCP サーバーは、AI エージェントと外部ツール (既存の API や npm パッケージなどのローカルツール) 間の通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを AI エージェントに依頼できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を利用できます。Web ページの移動、フォームの入力、ボタンの選択などを AI エージェントに依頼できます。

ほかにもさまざまなツールやリソースを提供する MCP サーバーがあります。GitHub はエコシステム内での発見と貢献を促す [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。使用する前にソースコードを慎重に確認し、発行元を検証して、セキュリティへの影響を考慮してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに `github` サーバーが設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトのテンプレートに含まれています。そのため、前の演習で Copilot は Issue のバックログを読み取ることができました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルへのインストールは不要です。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できるため、機能のテストに役立ちます。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、画面を表示せずに Playwright を実行します。デスクトップ画面のない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトでは、エンドツーエンドテストにすでに Playwright を使っているため、ブラウザーも通常インストール済みです。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、再度試します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを使うときにサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認し、サーバーの起動を許可します。

手動で起動する手順は省略できます。その場合、次の手順で Copilot が初めてツールを使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。開発者がアプリを操作して変更を確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、確認内容をまとめます。会話を離れずに機能が期待どおりに動作することを確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントは支援技術と同じように、ボタン、リンク、リスト項目などの構造化されたラベル付き要素を推論します。そのため、簡単な機能テストがアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを起動した状態で、次のプロンプトを Copilot に送信して、フィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。レポートを Issue の受け入れ基準と照らし合わせます。問題がある場合は、追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストを実行するには、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーのダウンロードが必要になる場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行するよう依頼して再試行します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したら、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミットメッセージを生成します。

   ![Copilot が行った変更を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドライン インターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot を使って PR を作成します。Copilot は関連する Issue を探し、Issue に関連付けられた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントドロップダウンで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを利用できるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報収集や操作の実行を Copilot が求めたら、必要に応じて **Continue** を選択します。GitHub ツールを初めて使うときに、GitHub への認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成したことを確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と MCP サーバーが Copilot にツールを追加する方法を確認しました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Playwright MCP を使って、リリース前にブラウザーでフィルター機能を手動でテストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、Copilot を拡張する方法は MCP だけではありません。次は、アクセシビリティレビューなどの特定タスクを効率化する[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
