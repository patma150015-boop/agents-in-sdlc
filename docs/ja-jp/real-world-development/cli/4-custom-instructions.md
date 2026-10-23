---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを完了する必要がある場合、その指針を Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も指定できます。フィルター機能を構築したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示ファイルとパスに応じた指示ファイルが、エージェントに適用される仕組みを確認します。
- コーディング標準を守るため、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手順に関するガイドラインと要件があります。

- コメントには、コードを言い換えるのではなく、意図や自明でない判断を記載します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TypeScript のドキュメントコメント (TSDoc) と JavaScript のドキュメントコメント (JSDoc) を付けます。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには、`Props` の契約を説明するドキュメントを付けます。関連コードが変わった場合はコメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使って Copilot に必要な情報を提供し、上記の手順に沿ってタスクを実行できるようにします。

## 指示ファイル

カスタム指示は、コーディングスタイルや要件を Copilot に伝えるコンテキストです。コーディング規約、ライブラリ、コメントの書き方などを指定すると、より適切な提案やコードを生成できるようになります。リポジトリ全体、または特定のファイル種別に指示を設定できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一のファイルです。多くのチャットや CLI リクエストに関わるプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、言語、ファイル種別、タスクごとのガイドラインを設定します。

> [!NOTE]
> ほかの指示形式とそのサポート状況は環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでにいくつかの指示ファイルがあります。変更を加える前に、現在のファイルを確認して、変更後の影響と比較できるようにします。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` フィールドを確認します。このフィールドには、指示を適用するファイルを定める glob が設定されています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。コアの `copilot-instructions.md` を変更して、新たに生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35行目付近にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、フィルター機能の会話を終了して再開すると、機能に関するコンテキストを保ったまま新しいガイダンスを読み込めます。

1. フィルター機能の会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択肢が表示されたら、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み込む仕組みを確認し、機能に新しい標準を適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される仕組みを確認しました。
- コーディング標準を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
