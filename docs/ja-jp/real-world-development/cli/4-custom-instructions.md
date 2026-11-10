---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、どのコードを作成するかだけでなく、どのように構成するかも指定できます。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準が守られるよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

優れた開発会社である Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントでは、コードの内容を言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。`db` 引数を注入する場合は、その引数も含めます。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連するコードが変わったらコメントも最新に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを実行するための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストと希望を Copilot に伝え、より適切な提案やコード スニペットを得られます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定の種類のファイルに対して指示を作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。多くのリクエストに関係するプロジェクト レベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイダンスを提供します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を使う前に、[カスタム指示の対応状況に関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでに指示ファイルが用意されています。変更前に内容を確認して、指示が与える影響を調べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、リポジトリで Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用先となるファイルを決める glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクト ファイルへのリンクを確認します。これにより、大きな指示を小さく再利用可能なファイルに分割し、Copilot が参照できる例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにあるファイルはよい出発点ですが、まだ不足があります。新たに生成する TypeScript に TSDoc コメントを追加するよう、コアの `copilot-instructions.md` ファイルを変更します。

1. `.github/copilot-instructions.md` で、35 行目付近にある **Code formatting guidance** というセクションを探します。
2. 最後の箇条書きに次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しい指示を使えるよう、指示を保存した後にフィルターの会話を終了して再開します。

1. フィルターの会話が開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   指示された場合は、フィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. 更新したガイダンスをコードに適用するよう、Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準が守られるよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
