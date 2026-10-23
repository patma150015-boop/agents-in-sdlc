---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここ数回の演習で、GitHub Copilot cloud agent にユーザー体験の改善や機能追加に関する3つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業を監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理する方法はいくつかあります。このミッションコントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を調べたり、進行中のセッションを調整して作業を導いたりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 実行中のセッションを調整し、機能を追加するよう依頼します。

## シナリオ

エージェントに高コントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイルを更新し、切り替え機能を追加する作業をすでに進めているため、同時に実装するのが合理的です。高コントラストに加えてライトモードも実装されるよう、エージェントの作業を調整します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左側のペインにあるタスクの一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新。
   - ゲームを変更するための API の生成。
   - Web サイトへの高コントラストモードの追加。
3. 実行中のタスクを1つ選択します。Copilot が行った次のような操作を確認します。
   - リポジトリからのコードのチェックアウト。
   - Copilot が作業するための環境の作成。
   - MCP サーバーのセットアップ。
   - 割り当てられたタスクを完了するための各種作業。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここから PR と変更されたファイルを確認し、作業をさらに監視できます。

## cloud agent に追加作業を依頼する

実行中のタスクを確認できたので、エージェントが高コントラストモードを実装している間に、ライトモードの切り替えも追加するよう Copilot に依頼します。

1. 高コントラストモードを追加しているセッションを選択します。タイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、数分間セッションを確認します。メッセージが表示され始めたら、作業開始の目安になります。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで、作業中の cloud agent セッションを調整するダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込んだことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が完了した後、新しい依頼を作業に取り込みます。

前と同じく、完了まで数分かかります。その間に、これまでに学んだ内容を振り返ることもできます。

## まとめと次のステップ

GitHub Copilot cloud agent の監視とガイダンスの中心となる Copilot agents ページを確認しました。このミッションコントロールでは、進行中や完了済みのタスクを追跡し、作業内容をレビューし、進行中のタスクの範囲を調整したり追加の指示を送ったりできます。

このレッスンでは、次のことを行いました。

- ミッションコントロールとエージェントページを確認し、cloud agent のタスクを監視しました。
- 実行中のセッションを調整し、追加機能を依頼しました。

Copilot がアクセシビリティ機能の作業を進めている間に、Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
