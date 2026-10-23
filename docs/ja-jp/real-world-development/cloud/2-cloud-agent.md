---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩む組織は少なくありません。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどが挙げられます。GitHub Copilot の cloud agent は、コードの更新や機能の追加などを自律的に実行します。エージェントは作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。面倒なタスクを任せることで開発を加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成に使う環境をカスタマイズします。
- 操作が安全に行われるようにします。
- 範囲を明確に定めた Issue の重要性を確認します。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、対応したい技術的負債があります。サイトの最初のバージョンを作成した委託業者はドキュメントを十分に整備しておらず、実質的に何もありません。まず、アプリケーション内のエクスポートされた関数すべてに TSDoc ドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。ゲームの詳細ページに同じカテゴリの関連ゲームを表示し、訪問者が引き続き閲覧できるようにします。まだ完成度の高い実装は必要ありません。ユーザー体験 (UX) の受け入れテストに使えるものを求めています。この作業は現在の阻害要因ですが、優先度がさらに高い Issue がほかにもあります。

このように、優先順位が下がりやすいタスクは Copilot cloud agent に割り当てるのに適しています。cloud agent に非同期で作業させている間、開発者は別のタスクに集中し、完了後に戻って結果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法で作業を依頼できます。割り当てると、Copilot は進捗を追跡するドラフトのプルリクエストを作成し、環境をセットアップして、タスクに取りかかります。実行中または完了後にセッションを確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 範囲を明確に定めた指示の重要性

GitHub Copilot に魔法のような機能があるわけではなく、考える作業を省略できるわけでもありません。一見単純な操作でも、詳細を調べると複雑な場合があります。そのため、[Copilot cloud agent に依頼するタスクの範囲を慎重に定めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階的に作業し、学び、試しながら調整します。生成 AI を使っても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と、Copilot の動作を導く方法を確認しました。カスタム指示の演習で `.github/copilot-instructions.md` を確認済みの場合は、そこでの設定を思い出してください。まだの場合は、今が簡単に確認するよい機会です。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその効果を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターを確認します。この指示は `**/*.test.ts` ファイルに適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストで TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーを確認します。後の演習で PR をレビューするときに、もう一度取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには適切な環境とセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同じで、Copilot はソフトウェアエンジニアのような手順で作業します。

cloud agent は、作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。演習を円滑に進められるよう、ラボではこの設定が事前に行われています。単一の Astro アプリ向けに、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを済ませた SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同様ですが、次の点に注目してください。

- `copilot-setup-steps` という1つのジョブがあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが認識していますが、多くのプロジェクトでは内容が古かったり、ドキュメント自体がなかったりします。この種の技術的負債は対応されないままになり、作業効率を下げ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成を得意とするため、Copilot cloud agent に割り当てるのに適した Issue です。cloud agent はバックグラウンドで必要なドキュメントを作成します。後の演習でその結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に新しい PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認できます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題に取り組む方法を検討します。

13. リストと実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ウィンドウを更新してください。

16. ライブセッションをスクロールし、Copilot が問題に取り組む様子を確認します。コードを調査して状態を把握し、適切な計画を検討し、コードを作成する過程を確認できます。

作業には数分かかる場合があります。Copilot cloud agent の主な目的の1つは、タスクを非同期に実行し、開発者が別の作業に取り組めるようにすることです。この機能を活用し、もう1つのタスクを cloud agent に割り当てた後、アプリケーションの機能を追加するコードの作成に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の1つは、作業を分担できることです。開発者があるタスクに取り組む間、エージェントは別のタスクを進められます。デザインチームが求める関連ゲーム機能に時間がかからないとしても、その時間を別の作業に使える場合があります。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい Issue のダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Issue に Copilot が作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同様に作業するため、完了するまで常に監視する必要はありません。完了後にレビューします。次は、カスタムエージェントの作成と使用に進みます。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]を使って、非同期で Issue を割り当て、作業を依頼する方法を確認しました。cloud agent は技術的負債への対応、新機能の作成、コードの別のフレームワークへの移行などに使えます。

この演習では、次のことを行いました。

- コード生成用の環境をカスタマイズしました。
- 操作を安全に実行する方法を確認しました。
- 範囲を明確に定めた Issue の重要性を確認しました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent 用セットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
