---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。具体的には、次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、フィルター機能の Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果をレビューして、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方法を定義し、Autopilot モードで構築してから、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を試しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、省略可能なクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成したもの

ワークショップでは、それぞれ更新済みの `main` から始める2つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、未評価を明示する状態をゲームカードに表示します。
2. **フィルター機能と品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業を1つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を介した Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA では、その結果に要件とテストカバレッジのレビューを組み合わせてから PR を作成しました。

追加したテストは実際の不足を解消するものである必要があります。QA でテストを追加する必要がなかった場合も、正しい結果になり得ます。マージを許可する前に、コードと検証の証拠を確認してください。変更後は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に渡すコンテキストとツールは、作業の結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに応じて調整できます。指示は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は構築前に方法を検討するとき、**Interactive** は対象を絞った変更を進めながら確認するとき、**Autopilot** は範囲が明確なタスクに使います。定型的な編集には速いモデルを、複雑な作業には高性能なモデルを選びます。

インフラストラクチャと同様に、コンテキストも重要です。何を、なぜ、どのように構築するかを明確に伝えると、結果が大きく変わります。

## さらに試す

中心的なワークフローを完了しました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

ツールを上達させるには、使い続けることが最善です。本番コードや趣味のコード、長年考えていたものの作る時間がなかった小さなアプリにも使ってみてください。学んだことをチームと共有し、チームからも学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張する場合は、省略可能な[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境で試す場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
