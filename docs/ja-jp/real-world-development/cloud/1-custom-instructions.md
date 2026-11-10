---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、ここではプロンプトの「変更前と変更後」を比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、その影響をリアルタイムで並べて示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供します。コードの提案を改善するために、コード内のドキュメントを標準に追加します。cloud agent は、このハーネスの後半で割り当てる issue に取り組むときに、この標準を参照します。

編集の前に、作業用のブランチを作成します。cloud agent は issue の対象ブランチから指示を読み取りますが、編集内容も整理してコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を提供するファイルです。リポジトリのコーディング標準に、コードの提案を改善するルールを追加します。

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

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、issue の対象ブランチにある指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を利用できるよう、変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。次の演習で割り当てる issue に取り組むとき、cloud agent はこれらの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業する場合は、ブランチを作成せずに `main` にコミットしてもかまいません。ここでは実際のプロジェクトに沿った進め方を紹介するため、ブランチを使っています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスで後ほど cloud agent が作成するプルリクエストをレビューするときに、その標準が適用されていることを確認します。

次は、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
