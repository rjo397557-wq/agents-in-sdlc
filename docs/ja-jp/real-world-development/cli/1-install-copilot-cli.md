---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルター issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディング アシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールとやり取りできます。すべてコマンド ラインから行えるため、タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップは、ツールのインストールです。すでに使い慣れているツールからインストールできます。

このレッスンでは、次のことを行います。

- npm を使って GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼して、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーを通じてフィルター issue を見つけます。

## シナリオ

チームは、増え続けるバックログに対応するため、AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に作業するターミナルで、エージェントの機能を使えるようにします。このレッスンでは、ワークショップの残りで利用できるように、インストールと認証を行います。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、device code の手順に従って認証し、Copilot CLI を認可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェル プロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI を起動した後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーを使って **Auto** を選択します。<kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで利用できるコマンドを確認します。<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、すべて動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトで、Copilot にフィルター issue を探すよう依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて issue を読みます。最初の小さな変更を完了した後に使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的に worktree を作成することはありません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、設定済みの権限以外でツールを使う前に確認を求めます。このワークショップでは `--yolo` を使って再起動します。ユーザーが承認したこのショートカットを使うと、Codespace 内で承認プロンプトが表示されなくなり、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL へのアクセスを許可する (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) 全自動権限を有効にします。Codespace や VM のような隔離された環境でのみ使い、日常の開発で使う既定のエイリアスにはしないでください。詳しくは[ツールの使用を許可および拒否する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を使って、後のレッスンで issue やプルリクエストを扱うための GitHub MCP の読み取り/書き込みツールを有効にします。Codespace によってローカル コンピューターへのアクセスは制限されますが、認証された GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` を入力して Copilot CLI を終了します。
2. リポジトリのルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて簡単な質問をして、会話が動作することを確認します。その後、`/exit` を入力して終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後に、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次のことを行いました。

- npm を使って GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼して、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを通じてフィルター issue を見つけました。

次は[最初の対象を絞った変更][next-lesson]を始め、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
