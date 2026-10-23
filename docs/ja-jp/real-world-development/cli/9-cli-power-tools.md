---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様、GitHub Copilot CLI にも、ツールを操作するためのスラッシュコマンドが用意されています。これらのコマンドを使うと、高度な機能、内部の状態、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を確認します。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の中心的なワークフローを完了しました。次は、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への省略可能な委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使えば、自分で状態を確認したり、圧縮したりできます。

1. Codespace に戻り、まだ開いていなければリポジトリのルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域に分けられたコンテキストを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が小さい場合は、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI は、コンテキストウィンドウがいっぱいになると自動的に圧縮します。タイミングを自分で選ぶ場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの使い方を向上できます。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで `/share` が提供するオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、目的に合った出力先を選びます。
3. エクスポートした内容を誰かに送信したり公開したりする前に確認します。エクスポートにはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの中心的な部分は完了です。Copilot CLI には、大きなタスクに役立つ次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに割り当て、並行して実行できます。
- `/worktree` は、別のタスク向けに分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送り、非同期で作業させます。エージェントがプルリクエストを作成する場合もあります。

追加の worktree やリモートでの作業が発生する可能性があるため、これらのコマンドは省略可能です。試す場合は、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の状態を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を確認しました。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

Copilot CLI にはほかにもスラッシュコマンドがあります。次は、[学んだ内容と今後のステップを振り返り][next-lesson]、このワークショップを締めくくります。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
