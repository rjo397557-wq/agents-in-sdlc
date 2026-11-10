---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な変更でも、通常は複数のファイルを更新し、テストを実行する必要があります。開発者は必要なファイルを探し、変更を行い、テストを実行して、デバッグし、更新し忘れたファイルを見つけ、さらに変更します。作業は次々に続きます。

Copilot Agent Mode が役立つのはこのような場面です。

Copilot Agent Mode は、IDE でより自律的に作業するよう設計されています。まず既存のプロジェクト構造を調べ、必要なファイルを更新し、テストなどのタスクを実行します。問題が見つかった場合は、自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は用途が異なります。Agent mode は IDE 内でタスクを実行し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチームの一員のように非同期で作業し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数のファイルにわたる変更を行う方法。
- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの数が増えてきたため、カテゴリと出版社で絞り込めるようにしたいと考えています。前の演習ではすでに出版社ヘルパーを追加しました。残りのデータ層、UI、テストの作業を Copilot Agent Mode で完了します。

## Tailspin Toys の Web サイトを実行する

変更を始める前に、Tailspin Toys の現在の機能を確認しましょう。

この Web サイトは、開発者向けのテーマを使ったボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示できます。単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページはフロントマターで Drizzle ORM を使ってローカル SQLite データベースを直接クエリします。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習から `main` ブランチにいるはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. フィルター機能の作業用ブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くかどうかを尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に移動します。Codespace で Web サーバーをホストするためのプライベート トンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホーム ページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリなどの情報を含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの最初の実装は動作しますが、新しい機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動作成されています。GitHub Copilot にそれらの issue を表示してもらいましょう。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** はファイルをまたぐ変更を自律的に計画して実装し、ターミナル コマンドを実行し、ツールを呼び出します。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者が特定のモデルを推奨しているわけではありません。ワークショップの作成に Claude Sonnet 4.5 を使ったため、手順にも記載しています。提案されるコードに一貫性を持たせ、スムーズに進められることを期待しています。ただし LLM は確率的に動作するため、ワークショップに記載された内容と異なる提案が表示されることがあります。これは通常の動作であり、想定されています。

> [!NOTE]
> LLM の確率的な特性により、Copilot が異なる MCP コマンドを使うことがありますが、タスクは同様に完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと思われる項目を優先順に並べたことを確認できます。

## 指示ファイルを確認する

Copilot にコードを生成させる前に、作業のコンテキストを提供する指示ファイルを確認します。Web サイトの機能を追加するときの方法を説明する [user interface (UI) ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別ごとの指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな指示セットを小さく分け、Copilot が参照できる例を示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも 3 つの別々の更新が必要です。

- データ層 (`src/lib/`) にフィルターのロジックを追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

また、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する画面の例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログにある関連 issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に必要なファイルを見つける様子を確認します。データ層ヘルパー、UI、テストを見つけて、ファイルを変更し、テストを実行します。

   ![Copilot がプロジェクト ファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行するなど、複数の作業を行います。タスクやコードベースの複雑さによって、数分かかることがあります。作業中、コード エディターに **Keep** と **Undo** が表示される場合があります。作業が完了すると、変更ごとに **Keep** または **Undo** を選べるようになります。作業の途中で選択する必要はありません。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に停止したり、テストが失敗したりすることがあります。問題ありません。Copilot は、タスクを完了してエラーを検出しなくなるまで、コード生成とテストを繰り返します。

   ![Copilot Agent Mode のチャット セッションが完了した状態](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot Agent Mode を使い、Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot Agent Mode が UI とデータ層にまたがる新機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調べ、関連ファイルを見つけて、複数のファイルにわたる変更を行う方法。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成しましょう。

### 追加の演習 — ページ分割を実装する

ゲーム一覧が増えるにつれ、ページ分割が必要になります。この演習で学んだことを使い、Copilot にサイトのページ分割を依頼してみましょう。次の点を検討してください。

- 指示ファイルを使うなど、既存のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページ分割の方法を決めます。
- 希望どおりの実装を生成できるよう、プロンプトで必要なガイダンスを Copilot に伝えます。
- 変更を依頼してコンテキストを提供するなど、GitHub Copilot と何度かやり取りする必要がある場合があります。これは Copilot を使うときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
