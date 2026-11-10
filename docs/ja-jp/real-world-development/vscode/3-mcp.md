---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

Agent mode は、エディターの外にアクセスできると、さらに多くのことができます。Copilot が外部ツールやサービスを使うための標準的な方法が Model Context Protocol (MCP) です。

![agent mode の内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントが外部のツールやサービスと通信できます。MCP によって、AI エージェントは外部ツールやサービスとリアルタイムにやり取りできます。最新情報（リソース）にアクセスしたり、ユーザーに代わって操作（ツール）を実行したりできます。

ツールとリソースは、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを通じて利用します。MCP サーバーは、AI エージェントと外部ツール（既存の API や NPM パッケージのようなローカル ツールなど）との通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。AI エージェントはリポジトリを作成、更新し、issue やプルリクエストを管理できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは Web ページに移動し、フォームに入力して、ボタンを選択できます。

ほかにも、多くの MCP サーバーが利用できます。GitHub は、エコシステム内の MCP サーバーを見つけやすくし、コントリビューションを促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。利用前にソース コードを慎重に確認し、発行者を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密性の高いリソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースが利用できる MCP サーバーを構成します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクト テンプレートに含まれているため、前の演習では Copilot が issue のバックログを読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使っているため、ローカルでインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot がブラウザーを操作できます。作成した機能をテストするために使います。

1. `.vscode/mcp.json` の `github` と並べて `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは Playwright を画面のないブラウザーで実行するよう指定します。Codespace には表示用デスクトップ環境がないため必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストで Playwright をすでに使っているため、通常、Playwright に必要なブラウザーはインストール済みです。ブラウザーが見つからないと Copilot が報告した場合は、`npx playwright install chromium` を実行してから再試行してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか尋ねられた場合は、構成を確認し、信頼するよう選択してサーバーを起動します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot がツールを使うときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。ユーザーがアプリを操作して確認する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を読み取って、確認内容をまとめます。会話を続けながら機能の動作をすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使って操作します。ボタン、リンク、リスト項目などのラベル付き要素を構造化してエージェントが判断します。支援技術と同じように情報を扱うため、簡単な機能テストでアクセシビリティも確認できます。

サーバーを接続してアプリを起動した状態で、次のプロンプトを Copilot に送信して、作成したフィルター機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを通じてブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件に照らしてまとめを確認してください。想定と異なる点があれば、追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには `http://localhost:4321` でアプリが動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Playwright MCP サーバーを初めて使うときに、ブラウザーのダウンロードが必要な場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能の動作を確認したので、チームがレビューできるようにプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更されたファイルが表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンド ライン インターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成も依頼しましょう。Copilot は関連する issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使用できるよう、エージェント ドロップダウンから **Agent** を選択します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を Copilot に許可します。GitHub ツールを初めて使うときに、GitHub への認証を求められることがあります。プロンプトに従って認証します。
5. Copilot が issue を検索して該当するものを見つけ、PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼しました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーで機能を**テスト**しました。Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティのレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
