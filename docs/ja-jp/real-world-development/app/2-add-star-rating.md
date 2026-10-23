---
title: "レッスン 2 - 星評価の追加で小さな成果を得る"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初の pull request としてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを使ったので、次は**エージェントセッション**を開始してプロジェクトに最初の変更を加えます。変更は小規模です。ゲームのデータにはすでに星評価が含まれていますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- エージェントセッションを開始し、セッションの構成を学びます。
- プロジェクトに小規模で対象を絞った変更を加えるようエージェントに依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定でき、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードには、タイトル、カテゴリー、パブリッシャー、説明だけが表示されています。最初のセッションの準備運動として、各カードに既存の評価を表示するようエージェントに依頼します。小規模で自己完結した、最初のセッションに最適な変更です。

## セッションの構造

**セッション**とは、独立したワークスペースで実行されるエージェントとの会話です。各セッションには専用の Git worktree とブランチが割り当てられるため、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにグループ化されてサイドバーに表示され、選択すると切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調査および編集するときのエージェントの**ツールアクティビティ**、差分付きの**変更済みファイル**一覧という3つの要素が表示されます。

## セッションを開始して変更を依頼する

[前のレッスン][prior-lesson]では、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用の新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻ります。アプリを閉じている場合は開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. **new working tree** と、プロンプトボックスの下にある **Interactive** モードを選択します。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は最初に、新しい分離環境である worktree を作成します。次にプロジェクトを調査し、必要なファイルを更新して機能を実装します。

## 差分をレビューする

人工知能 (AI) が生成したすべての変更は、どれほど小さくてもマージ前にレビューする必要があります。Copilot app 内で変更を確認します。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。次のように、評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" を表示する小さなブロックが追加されているはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に確率的に動作するため、実際のコードは上記と異なる場合があります。ただし、比較的よく似たものになります。

## 変更を確認する

ブラウザーを開く前に、エージェントの自動チェック結果をレビューします。ブラウザー確認に進む前に、数値の `starRating` と `null` の場合の代替表示をテストしていることを確認してください。

コードを読むだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、ブラウザーキャンバスで開いて更新された画面を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べ、自分でも作成しますが、ここでは組み込みのブラウザーキャンバスを使用します。

1. 自動チェックの結果をレビューし、数値の `starRating` と `null` の場合の代替表示をテストしていることを確認します。前提条件の不足やスキップされたチェックは成功ではありません。インストールを求められた場合は、内容を確認してから承認します。
2. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

3. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
4. 評価済みのゲームカードに5点満点の値が表示されることを確認します。
5. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初の pull request を作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするための pull request (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. Copilot が、リポジトリのポリシーにより PR の作成前に対応する Issue が必要だと説明した場合は、そのプロセスに従うことを確認します。
4. Copilot が PR の作成を開始します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開いて確認します。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェントセッションを開始し、セッションの構成を学びました。
- ゲームカードに小規模で対象を絞った変更を加えるようエージェントに指示しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして、明示的にマージしました。

次は、[フィルター機能の Issue から開始し、Plan モードと Autopilot モードを使用して][next-lesson]、より大規模な機能を構築します。

## リソース

- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app での Issue と pull request の管理][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして設定する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
