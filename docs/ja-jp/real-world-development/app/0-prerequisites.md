---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub を一元的に扱うデスクトップアプリです。Issue とプルリクエストにすばやくアクセスでき、GitHub Copilot を使って開発できます。このワークショップでは、Astro で構築された Tailspin Toys アプリを更新しながら、自分のコンピューター上で作業します。始める前に、ローカル環境に Node.js がインストールされていることを確認し、その後 Copilot app をインストールします。

このレッスンでは、次のことを行います。

- コンピューター上でプロジェクトのテストを実行できるように Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を構築させ、Tailspin Toys のテストスイートをローカルで実行します。そのためには、プロジェクトが必要とする唯一のランタイムである [Node.js][nodejs] が必要です。現在の長期サポート (LTS) リリースをインストールしてください。

どのプラットフォームでも、公式インストーラーを使うのが最も簡単です。

1. Windows Terminal、macOS のターミナル、または普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、Node.js と npm のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js がプロジェクトの README と `package.json` に記載されている要件を満たしている場合は、次のセクションに進みます。

> [!TIP]
> Node.js がインストールされていない場合や、更新が必要な場合にのみ、以下の手順を実行してください。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用しているオペレーティングシステム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では、**Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルウィンドウを開きます。
8. 新しいターミナルで次のコマンドを実行して、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールされた Node.js と npm のバージョンが表示されることを確認します。

> [!IMPORTANT]
> Copilot app のエージェントからも `node --version` と `npm --version` を実行できる必要があります。通常のターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了してから再起動してください。セッションでどちらかのコマンドが引き続き失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]に従います。

> [!IMPORTANT]
> エンドツーエンド (E2E) チェックを実行するには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree の準備については Tailspin Toys リポジトリの README に従い、インストールを求められた場合は、許可する前に内容を確認してください。

## 演習用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレートリポジトリ][template-repository]からコピーを作成してください。新しいリポジトリにはラボに必要なすべてのファイルが含まれており、次のレッスンでアプリに接続します。

1. 新しいブラウザーウィンドウで、このラボの [GitHub リポジトリ](https://github.com/github-samples/tailspin-toys)を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、**Create a new repository** を選択して、リポジトリの自分用コピーを作成します。

    ![Use this template ボタンのドロップダウンで Create a new repository が選択されている画面](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従います。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

    ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 後でラボ内で参照するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの Issue を使うため、自分で作成する必要はありません。

ワークショップ用テンプレートの新しいコピーを使用してください。リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、既存のキャンバス拡張機能が含まれています。ワークショップ中にスキルをカスタマイズし、QA エージェントを作成します。古いコピーを使う場合は、必要なファイルが含まれているか進行役に確認してください。

## まとめと次のステップ

準備が整いました。このレッスンでは、次のことを行いました。

- プロジェクトを自分のコンピューター上でビルドおよびテストできるように Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は、[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースを確認します。

## リソース

- [Node.js のダウンロード][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
