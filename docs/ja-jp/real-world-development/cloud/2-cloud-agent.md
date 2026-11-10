---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

テクノロジー関連の負債に悩まされていない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残り続ける機能リクエストなど、さまざまな問題があります。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。エージェントの作業が完了すると、人間の開発者がレビューできるドラフト PR が作成されます。面倒な作業を任せて開発を加速し、より大きな課題に集中できます。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- スコープを明確にした issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。サイトの初期バージョンを作成した開発業者が、ドキュメントを十分に整備していませんでした。実際、ほとんどドキュメントがありません。まず、アプリケーションのエクスポートされた関数に TSDoc のドキュメント コメントを追加したいと考えています。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示し、ユーザーがほかのタイトルも続けて閲覧できるようにします。まだ仕上げられた実装は必要なく、UX の受け入れテストに使えるものを求めています。この作業は現時点でブロッカーですが、より優先度の高い issue がほかにあります。

どちらも、優先順位が下がりやすいタスクです。Copilot cloud agent に割り当てると、非同期で処理される間に別の作業に集中でき、後から戻って作業をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][start-copilot-sessions]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフト プルリクエストを作成し、環境をセットアップして、タスクに取りかかります。処理中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## スコープを明確にした指示の重要性

GitHub Copilot に魔法はありません。考える手間を省けるわけではありません。一見簡単な操作でも、詳しく調べると複雑さがあります。[Copilot cloud agent に依頼するタスクのスコープ][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に進め、学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と、Copilot のガイドに使う方法を紹介しました。カスタム指示の演習をすでに行った場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、今のうちに簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でそれらの効果を見分けられるようにします。

リポジトリの github.com の Web UI、または Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。ここでは、指示が `**/*.test.ts` に適用されます。

次のセクションで **Code lacks documentation** issue を cloud agent に割り当てたら、作成されたプルリクエストで TSDoc コメント、TypeScript の規約、コメント ヘッダーを確認してください。これらの指示ファイルが反映されています。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

誰がコードを書く場合でも、通常は環境を準備し、正しい状態であることを確認するセットアップ スクリプトを実行します。Copilot にタスクを割り当てる場合も同じです。Copilot は SWE と同じように作業します。

Cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。ラボの流れを整え、学習に集中できるよう、あらかじめ設定済みです。単一の Astro アプリで使う Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを済ませた SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` というジョブが 1 つあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古かったり、そもそもなかったりします。この種の技術的負債は放置されやすく、開発の生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成に役立つため、このタスクは Copilot cloud agent に割り当てるのに適しています。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でその作業をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の文を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト（PR）を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザー ウィンドウを更新してください。

16. セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調査して状態を把握し、計画を考えて、コードを作成するまでの流れを見られます。

通常、この処理には数分かかります。Copilot cloud agent の主な目的は、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。もう 1 つのタスクを cloud agent に割り当てたら、アプリケーションに機能を追加する作業を進めましょう。

## ゲームの詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチーム向けの関連ゲーム欄を追加する作業が短時間で終わるとしても、その時間をほかのタスクに使えます。次のタスクを Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の文を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

  ![issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が issue の最初のコメントに表示されます。

![Copilot が issue の作業中であることを目の絵文字で示す画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組み始めました。Copilot cloud agent は SWE と同じように動作するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、issue を Copilot に割り当てて非同期に実行する方法を学びました。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などを依頼できます。

次の内容を確認しました。

- コード生成環境をカスタマイズする方法。
- 操作を安全に行う方法。
- スコープを明確にした issue の重要性。
- issue を Copilot に割り当てる方法。

cloud agent がバックグラウンドで作業する間、次はカスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで扱ったカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
[start-copilot-sessions]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
