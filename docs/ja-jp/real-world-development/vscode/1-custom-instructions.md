---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。コード生成の方法を指示できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を適用したまま、フィルター機能用の最初のデータ処理部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトを再実行し、新しい標準が生成コードに反映されることを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、Copilot が次の演習で利用できるようにします。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習の目的は、出力を文字単位で一致させることではなく、指示の更新後に動作がどのように変化するかを確認することです。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手順に関するガイドラインと要件があります。

- データレイヤーには必ず単体テストを作成します。
- UI はダークモードにし、モダンな印象にします。
- TSDoc ドキュメントコメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って Copilot に必要な情報を提供し、これらの手順に沿ってタスクを実行できるようにします。

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

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを適用した状態でプロンプトを送ります。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. Copilot が必要なコンテキストだけを読み込めるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、ヘルパーファイルとテストなどのコードを更新します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、発行元の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。ここで確認するのは、指示の更新後に一貫性が改善する傾向です。

## リポジトリ全体に適用する標準を追加する

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

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用するよう導き、次の演習で使用する実際のコードに反映しました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリのほかのファイルと同様に、ソース管理で管理します。指示の変更とブランチをコミットして、リポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の変更とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで指示ファイルと、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。手動でも作成できますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼します。Issue を検索し、Issue をクローズする PR を作成してから、マージするように依頼します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub の操作を依頼します。プロジェクトのテンプレートでは GitHub のツールがワークスペースに設定済みのため、準備作業はありません。Copilot が最初にツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続の仕組みである Model Context Protocol について学びます。

1. Codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、新しい発行元ヘルパーの基盤を説明に含めます。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作を実行するために **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、Copilot に PR のマージとローカルブランチを `main` に戻すことを依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドを実行するために **Allow** を求めたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援で、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み込む仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示がある状態でコード生成のプロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加しました。
- 同じプロンプトを再実行し、提案コードに新しい標準が反映されることを確認しました。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で利用できるようにしました。

次は、Copilot がコードベース全体に新機能を追加するときに、[エージェントモードで指示を活用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示をよりよく書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルとその他のリソース][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
