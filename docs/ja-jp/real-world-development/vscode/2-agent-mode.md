---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの小さな更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は必要なファイルを探し、変更し、テストを実行し、デバッグし、変更し忘れたファイルを探して、さらに更新します。作業は次々と続きます。

ここで役立つのが Copilot Agent Mode です。

Copilot Agent Mode は IDE 内でより自律的に動作します。開発者と同じように既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode で Web サイトに機能を追加する方法を確認します。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で作業するため、短いフィードバックサイクルと対話的な操作が可能です。cloud agent はチームのペアプログラマーとして非同期に作業し、Issue やプルリクエストを通じて開発者とやり取りします。

この演習では、次のことを行います。

- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて連携した変更を加える方法を確認します。
- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装します。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加したので、残りのデータレイヤー、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを起動する

変更する前に、Tailspin Toys の Web サイトを見て、現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲーム一覧を表示し、個々のゲームの詳細を表示できます。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページは frontmatter 内で Drizzle ORM を使い、ローカルの SQLite データベースに直接クエリします。独立したバックエンド API やクライアントサイド UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、必要に応じて標準 DOM API を使う小さなスコープ付き Astro `<script>` でインタラクティブ機能を追加します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。GitHub Codespace では次の手順で実行します。

1. Codespace に戻ります。前の演習の後であれば、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルター機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザーウィンドウを開くダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルの Astro サーバーアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace へのプライベートトンネルで、Web サーバーが Codespace 上で実行されます。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧とタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、発行元、カテゴリなどの詳細を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加したいと考えています。まず、バックログを確認します。テンプレートからプロジェクトを設定したとき、Issue のバックログは自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。**Agent** エージェントは自律的に計画を立て、複数ファイルにわたる変更を実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップの作成時に Claude Sonnet 4.5 を使ったため、説明にも記載しています。提案コードに一貫性を持たせ、スムーズに演習を進められるようにしています。ただし、LLM は確率的に動作するため、ワークショップの説明とは異なる提案が表示される場合があります。これは正常で、想定される動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送り、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーにとって役立つと思われる順に項目を優先付けしたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に与える指示ファイルを確認します。Web サイトへの機能追加の方法を説明する[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、各ファイル種別向けの指示へのリンク。

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、複雑な指示を再利用可能な小さな単位に分け、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の3つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルター処理を追加または改善します。
- フィルターの動作に関するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、すべて成功することを確認します。Copilot Agent Mode にこれらのタスクを依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログにある関連 Issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、機能に関係するファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを見つけてから、ファイルの変更とテストの実行に進みます。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行います。タスクとコードベースの複雑さによって、数分かかる場合があります。その間、コードエディターに **Keep** と **Undo** が表示されることがあります。作業中に選択する必要はありません。完了すると、すべての変更に **Keep** または **Undo** が表示されます。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat のダイアログでテスト実行の確認を求める画面](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot を含む AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode で Tailspin Toys の Web サイトに機能を追加する方法を確認しました。この演習では、次のことを行いました。

- GitHub Copilot Agent Mode で UI とデータレイヤーにまたがる新機能を実装しました。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを見つけて連携した変更を加える方法を確認しました。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューしました。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだことを使って、Copilot に Web サイトのページネーション実装を依頼します。次の点を考慮してください。

- 既存の指示ファイルなど、ベストプラクティスに従います。
- ユーザーがページサイズを選択できるようにするか、固定するかなど、ページネーションの方法を検討します。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- Copilot にコンテキストを提供し、変更を依頼しながら反復する場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
