---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同じように、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数用意されています。コマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ほかの便利なコマンドを確認しましょう。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI の主要なワークフローが完了しました。次は、コンテキストの管理、モデルの切り替え、セッションの共有など、追加機能を確認します。[Copilot cloud agent][about-cloud-agent] に作業を委任することもできます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使ってコンテキストを自分で確認したり圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を実行して、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別タスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使いたいモデルの一覧を表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールの活用方法を互いに改善できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートし、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、用途に合う方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を並行して進める、または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には大きなタスク向けのコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に独立した Git worktree を作成します。
- `/delegate` は、非同期で作業し、プルリクエストを開くこともある Copilot cloud agent にタスクを送信します。

これらのコマンドは追加の worktree やリモートの作業を作成できるため、任意です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューします。非同期エージェントについて詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI でスラッシュコマンドを使うと、設定を変更し、セッションを共有し、内部で行われている処理を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI にはほかにもスラッシュコマンドや機能があります。次は[ここまでの学びを振り返り][next-lesson]、さらに学ぶための次のステップを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
