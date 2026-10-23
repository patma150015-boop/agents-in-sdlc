---
title: "レッスン 1 - GitHub Copilot app のインストール"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップアプリケーションです。GitHub Copilot CLI (コマンドラインインターフェイス) を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、継続的インテグレーション (CI) パイプラインをすぐに利用できます。複数のエージェントをそれぞれ分離されたワークスペースで並列に動かし、繰り返し発生するタスクを自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーを準備できたので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートが用意したバックログを含め、ワークスペースを確認します。
- クイックチャットでアプリについて質問します。

## シナリオ

チームは、増え続けるバックログに対応するために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールしてプロジェクトに接続し、プロジェクトについての会話を始められるようにします。

> [!NOTE]
> Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) のいずれかが必要です。Copilot Business または Copilot Enterprise を使用している場合、管理者が **GitHub Copilot app** ポリシーを有効にしておく必要があります。

## GitHub Copilot app をインストールして設定する

GitHub Copilot app を使うには、まずアプリをインストールします。Windows、macOS、Linux 向けのバージョンがあります。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の指示に従って認証します。データ所在地を設定した GitHub Enterprise Cloud を使う場合は、**Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` のホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディング中にリポジトリの選択を求められたら、テンプレートから作成した Tailspin Toys リポジトリを選択します。選択をスキップして、オンボーディング後にプロジェクトを追加することもできます。
6. テーマを選び、オンボーディングを完了します。
7. サイドバーの **Sessions** の下にリポジトリが表示されることを確認します。すでに一覧にある場合は、追加する必要はありません。
8. リポジトリがまだ接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者名とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されることを確認します。ここで使用するレイアウトでは、`tailspin-toys` などのリポジトリフォルダーが **Sessions** の下にまとめられ、各リポジトリの下にセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、アプリの各領域を確認します。サイドバーは、主に次の領域で構成されています。

- **New**: 新しいセッションを開始するか、簡単な質問には **Chat** を選びます。
- **Pull requests**: アプリのネイティブ GitHub 統合を通じてプルリクエストを確認します。一覧の参照や絞り込み、CI 状態の確認、レビューをアプリ内で行えます。
- **Issues**: Issue を確認します。一覧の参照や絞り込み、Issue からのセッション開始ができます。
- **Automations**: スケジュールまたはオンデマンドで実行する、保存済みのエージェントタスクです。To Do リストや定期的なプロジェクト保守など、手間のかかる作業を任せられます。まとめでは次のステップとして紹介しますが、ワークショップ内の演習ではありません。
- **Customize**: Model Context Protocol (MCP) サーバー、プラグイン、スキル、その他のコンポーネントで Copilot app に機能を追加します。Playwright MCP の設定で使います。
- **Chats**: 独自のブランチやワークスペースを必要としない、質問やブレインストーミング向けの簡単な会話です。最初のチャットを開始すると表示されます。
- **Sessions**: リポジトリフォルダーと、そのエージェントセッションをまとめます。`tailspin-toys` などのリポジトリ名を選ぶとリポジトリビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは独立したワークスペースで実行されるため、変更を競合させずに複数のセッションを同時に実行できます。星評価を追加するときに最初のセッションを開始します。

ワークショップを進めながら、ワークスペースの機能を確認していきます。

> [!TIP]
> 迷ったときは Copilot に質問してください。操作方法が分からない場合や、何かが可能かどうか知りたい場合は、Copilot が案内してくれます。

### 用意されたバックログを確認する

ほとんどのプロジェクトにはバックログがあり、Tailspin Toys も同様です。テンプレートから作成したときに用意されたバックログを確認します。

1. サイドバーの **Sessions** で、Tailspin Toys のリポジトリフォルダーを選択します。フォルダーの下にある個別のセッションではありません。次に、リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を決めつけず、次のタイトルを探します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Issue を選択して詳細を読みます。各 Issue はエージェントセッションの開始点にもなります。最初の小さな変更を完了した後、フィルター機能の Issue から作業を開始します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。ほかのリポジトリの作業を表示するには、そのリポジトリもアプリに追加します。

## クイックチャットを試す

アプリに慣れるには、アプリ自体について質問するのが効果的です。その用途には**クイックチャット**が適しています。ブランチや worktree を作らずに質問やアイデア出しができるため、セッションを必要としない短い質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** を選択して最初のチャットを始めます。その後 **Chats** が表示されます。
2. アプリにセッションの仕組みを尋ねます。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは独立した Git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して実行できます。会話は続けることも、いつでも新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。次のことを行いました。

- アプリをインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで、その場限りの短い質問をしました。

次は、[最初のエージェントセッションを開始し][next-lesson]、ゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使い始め方][getting-started]
- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
