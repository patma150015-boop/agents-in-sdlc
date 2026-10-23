---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善や機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザインチームが改善を重ねるための関連ゲーム機能の構築、高コントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送り、作業を改善します。

### シナリオ

生成 AI を使っても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って進める必要があります。ここでは、チームによるレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監視なしで非同期に作業するため、安全を保つためにいくつかのセキュリティ制約があります。

- Copilot はリポジトリに対する読み取りアクセスのみを持ち、コード用に使うブランチに対してのみ書き込みアクセスを持ちます。
- cloud agent は GitHub Actions 内で実行され、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、コードにドキュメントを追加するため GitHub Copilot cloud agent が作成した1つ目のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたことを示す警告が表示される場合があります。外部 MCP サーバーへの呼び出しなど、Copilot の外部リソースへのアクセスは既定で制限されているため、これは想定どおりです。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択し、プルリクエストの一覧を開きます。
3. **Add missing documentation** または同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は Copilot が作業を終えるまで待ちます。完了まで数分かかることがあるため、休憩したり、これまでに学んだ内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc ドキュメントコメントなど、更新されたコードを確認します。実際の変更内容は異なる場合があります。

   変更を確認するときは、演習 2 の最初に確認した指示ファイルに基づく TSDoc ドキュメントコメント、TypeScript の規約、コメントヘッダーに注目します。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 承認を待っているワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックは成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チームメンバーと同じように、プルリクエストのコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを読み、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼する文面を一律に指定することはできません。次のような変更を提案できます。

- 各コードファイルの先頭に、ファイルの役割を短く説明するコメントヘッダーを追加します。
- TypeScript と Astro のファイルに TSDoc ドキュメントコメントを追加します。
- Astro アプリの構造を説明する README を作成します。

1. 上記の例や、コードベースに追加したい別のドキュメントの提案を使い、**@copilot** をメンションして生成されたドキュメントの変更を依頼します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。更新用に新しいセッションが始まることに注目してください。
3. **Back to pull request** を選択して PR に戻ります。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが追加されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復して変更を依頼できます。準備ができたら、PR をドラフトからレビュー準備完了に変更し、`main` ブランチにマージできます。

![PR をレビュー準備完了に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次に、ゲーム詳細ページに関連ゲームを表示する Issue に対して Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. 承認を待っているワークフローがあることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックは成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えます。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼し、反復して改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc ドキュメントコメントを追加したりできます (この Issue を割り当てた時点では、カスタム指示の更新前でした)。前の PR と同様に、**Conversation** タブにコメントを追加して依頼すると、Copilot が読み取り、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントで実装した機能をレビューします。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 でミッションコントロールから追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR のタイトルに **[WIP]** が付きます。その場合は Copilot が作業を終えるまで待ちます。完了まで数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持するためのローカルストレージの使用。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボードナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. 承認を待っているワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェックセクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一 Astro アプリのプロジェクトチェックは成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に `<branch-name>` を置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動してブラウザーで高コントラストモードとライトモードの切り替えをテストし、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装するよう Copilot を導いたことを確認します。アクセシビリティに関する懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** にメンションして更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent は、実際のバックログ項目を任せて、開発者が別の作業に取り組めるときに最も役立ちます。この習慣を身に付けるため、リポジトリに追加の Issue を作成して Copilot に割り当ててみましょう。たとえば次のようなタスクがあります。

- ゲーム詳細ページに支援に関心のある人向けのフォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、後で cloud agent の出力に与える影響を確認できるようにしました。**
- **Issue を Copilot cloud agent に割り当て、環境を準備し、計画して非同期に実行する様子を確認しました。**
- **アクセシビリティ用カスタムエージェントを作成して使用し、高コントラストモードとライトモードの切り替えを追加しました。**
- **Copilot agents ページをミッションコントロールとして使い、実行中のアクセシビリティセッションを監視して調整しました。**
- **cloud agent のプルリクエストをレビューして反復的に改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## 振り返りと次のステップ

Cloud agent ハーネスを完了しました。引き続き学ぶ場合は、ほかのハーネスで今回の内容を別の方法で試せます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` による cloud agent への連携を使って、ターミナルから2つの変更をレビューしてリリースします。

自分のリポジトリでは、次のような作業を試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 異なる技術スタック用に `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent 用セットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
