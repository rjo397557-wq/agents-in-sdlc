---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。コード生成の指針を提供できます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- 既存の指示を適用した状態で、フィルター機能の最初のデータ スライス（出版社ヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認します。
- 指示の変更と機能の一部を `main` にコミットし、Copilot が次の演習で新しい指針を使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準に沿わない場合もあります。Copilot は確率的に動作します。この演習では、出力を一字一句同じにするのではなく、指示を更新した後に動作の傾向が変わることを確認します。

## 指示ファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメント コメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと希望を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot に送信する多くのチャットや CLI リクエストに関係する、プロジェクト全体のコンテキストを記述します。技術スタック、開発対象の概要、ベスト プラクティス、全般的な指針などが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイルの種類に応じて作成できます。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの作成といったタスクのガイドラインを記述できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準形式の指示ファイルにも対応しています。[対応する指示ファイル][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベスト プラクティス

このワークショップでは指示ファイルの作成について詳しく扱いませんが、サンプル プロジェクトには代表的な方法が含まれています。大まかな指針は次のとおりです。

- `copilot-instructions.md` には、開発対象、プロジェクトの構造、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記述します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層などのファイル種別や、特定のタスクに関する指示を記述します。
- 自然な言葉を使い、明確なガイダンスにします。コードをどのように書くべきか、またどのように書くべきでないかの例を含めます。

指示ファイルの作成に唯一の正解はありません。AI の使い方と同じように、実際に試し、プロジェクトやシナリオに合う方法を見つけてください。

> [!TIP]
> GitHub Copilot を使う各プロジェクトには、充実した指示ファイルを用意することをお勧めします。このプロジェクトでは、[UI 更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けの指示ファイルを確認できます。
>
> Copilot に指示ファイルを生成してもらうこともできます。各環境で手順は異なります（例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`）。必要な場合は、利用中の環境向けのレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** にある **GitHub Actions Workflows** の指針も確認してください。これらは Copilot とやり取りするときに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対パスに対する glob で、指示の適用先を決めています。たとえば `**/*.test.ts` に一致するすべての TypeScript テスト ファイルに適用されます。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの別の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さく再利用可能なファイルに分割し、Copilot が参照できる例を示せます（ここにあるパスはリポジトリのルートではなく、指示ファイルからの相対パスです）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更を行うブランチを作成する

ここではベスト プラクティスに沿って、作業用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

4. Copilot に次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べ、コードとテストを含むファイルを更新します。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、型付きの出版社配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 提案されたコードに TSDoc のドキュメント コメントとファイル コメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の向上を確認することが、この演習の目的です。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。TSDoc のドキュメント コメントとファイル コメント ヘッダーを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリ レベル（`.github/copilot-instructions.md`）で作成して全体に適用することも、特定の言語、ファイル種別、タスク向けに `*.instructions.md` として作成することもできます。今回追加したドキュメント コメントのルールのような、プロジェクト全体の標準にはリポジトリ レベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に正しいファイルを示します。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習で利用する実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じように、ソース管理で管理します。変更をコミットしてリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、指示ファイルとヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示ファイルと、ヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。手動で行う代わりに、Copilot に issue を検索し、issue を完了する PR を作成して、マージするよう依頼します。

> [!NOTE]
> この演習では、Copilot に GitHub 上の操作を依頼します。プロジェクトのテンプレートでは、GitHub ツールがすでにワークスペースに接続されています。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められることがあります。プロンプトに従って認証してください。後の演習では、この接続に使われる Model Context Protocol について学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を検索し、現在のブランチから PR を作成して、指示の更新と出版社ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. Copilot に GitHub 操作の許可を求められたら、コマンドを確認して、適切な場合は **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ローカル ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に GitHub 操作とシェル コマンドの許可を求められたら、コマンドを確認して、適切な場合は **Allow** を選択します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

この演習では、Copilot が指示ファイルからコンテキストを取得する仕組みを確認しました。続いて Copilot Chat in VS Code を使い、次のことを行いました。

- コード生成のプロンプトを送り、既存の指示を使ったときの出力を確認しました。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加しました。
- 同じプロンプトを再実行し、新しい標準が提案に反映されることを確認しました。
- 次の演習で作業を引き継げるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は [agent mode][next-lesson] を使い、コードベース全体にわたる新機能を Copilot に追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
