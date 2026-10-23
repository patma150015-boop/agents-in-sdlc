---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行するかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムで並べて影響を示すのは難しいからです。指示ファイルの影響は、このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手順に関するガイドラインと要件があります。

- データレイヤーには必ず単体テストを作成します。
- UI はダークモードにし、モダンな印象にします。
- TSDoc ドキュメントコメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って Copilot に必要な情報を提供し、上記の手順に沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示は、コーディングスタイルや要件に関するコンテキストや設定を Copilot に伝えます。コーディング規約、ライブラリ、コメントの好みなどを指定して、関連性の高い提案やコードを生成するよう導けます。リポジトリ全体、または特定の種類のファイルに対する指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一のファイルです。技術スタック、開発中の内容の概要、ベストプラクティス、全般的なガイダンスなど、多くのチャットや CLI リクエストに関連するプロジェクト情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に指針を設定します。TypeScript や Astro などの言語、UI コンポーネントや単体テストの作成などのタスクに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には適用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルを使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも対応しています。[カスタム指示のサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することは、このワークショップの範囲外です。ここでは、サンプルプロジェクトのファイルを参考に、基本的なアプローチを紹介します。

- `copilot-instructions.md` には、開発中の内容、プロジェクトの構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データレイヤーなど、特定のファイル種別やタスクに関する指示を記載します。
- 自然な言葉で分かりやすく説明し、コードのよい例と悪い例を示します。

指示ファイルの作成方法や AI の利用方法に、唯一の正解はありません。プロジェクトで試しながら、最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク用のファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。操作はインターフェイスによって異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。必要に応じて、このワークショップの手順で説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェント、その他のリソースが集まった [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリにある指示ファイルを確認します。コアの `copilot-instructions.md` と、さまざまなタスクに対応した `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのすべてのやり取りに関係します。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開き、冒頭の `applyTo` フィールドを確認します。このフィールドには、リポジトリのルートからの相対パスで、指示が適用されるファイルを指定する glob が設定されています。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトで単体テストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。このように大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考例を示せます (ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する指示はありません。次の手順で、TSDoc ドキュメントコメントとファイルヘッダーの規則を追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、コード内に TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーを記載するよう定める、プロジェクト全体のルールです。後の演習で Issue を割り当てるときに、cloud agent がこの指示を使います。

編集を始める前に、作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み込みますが、ここでは変更をきちんとコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開き直します。
2. 27行目付近にある **Code formatting requirements** セクションを探します。ここにはコーディング標準が記載されていますが、コード内のドキュメントに関する規則がありません。そのため、生成されたヘルパーにはドキュメントコメントがありませんでした。
3. 既存の標準の直後に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで説明したように、指示ファイルはリポジトリ全体のガイダンスを設定する `.github/copilot-instructions.md` と、特定の言語、ファイル種別、タスクを対象にした `*.instructions.md` ファイルに分けられます。今回追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。そのため、cloud agent に指示を適用させるには、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチの作成を省略して `main` に直接コミットできます。ここでは、実際のプロジェクトでの作業方法に沿うため、ブランチを作成しています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み込む仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半では、cloud agent が作成するプルリクエストでこの標準が適用されることを確認します。

次は、最初の Issue を Copilot cloud agent に[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
