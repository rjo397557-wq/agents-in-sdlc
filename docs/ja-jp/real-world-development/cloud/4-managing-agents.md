---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、Copilot cloud agent に、ユーザー エクスペリエンスの改善や機能の追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクの進行状況を監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[agents ページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。エージェントの作業を調べたり、実行中のセッションに追加のガイダンスを与えたりできます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼します。

## シナリオ

チームはエージェントにハイコントラスト モードを作成するよう依頼した後、ライトモードも追加するよい機会だと気付きました。サイトのスタイル更新と切り替え機能の作業をすでに進めているため、ライトモードも含めることにしました。エージェントの作業に方向付けを行い、ハイコントラストに加えてライトモードも実装します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左側のペインでタスクの一覧を確認します。次のようなタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを変更する API を生成する。
    - Web サイトにハイコントラスト モードを追加する。
3. 実行中のタスクを 1 つ選択します。Copilot が行った操作を確認します。次のような操作が含まれます。
    - リポジトリのチェックアウト。
    - Copilot が作業する環境の作成。
    - MCP サーバーのセットアップ。
    - 割り当てられたタスクを完了するための各種手順。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や選択した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認できます。

## cloud agent に方向付けする

実行中のタスクを確認したので、ハイコントラスト モードを作成中の Copilot にライトモードの切り替えも追加するよう依頼します。

1. ハイコントラスト モードの追加に関するセッションを選択します。正確なタイトルは Copilot が付けた名前や作業状況によって異なります。

    ![Mission control に表示されるアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、エージェントが作業を開始するまで数分待ちます。次のようなメッセージが表示されたら、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページで cloud agent タスクの進行中に追加指示を入力するダイアログ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映することを確認します。

## Copilot の作業を待つ

これまでと同様に、更新されたタスクに Copilot が取り組みます。メッセージを送信した時点で作業中の手順を終えると、新しい依頼が作業に組み込まれます。

今回も数分かかります。その間、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページを使い、GitHub Copilot cloud agent のタスクを監視してガイドする方法を学びました。この mission control では、実行中や完了済みのすべてのタスクを追跡し、作業内容をレビューし、進行中のタスクに追加の指示を送れます。

この演習では、次の内容を学びました。

- mission control と agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を完成させる間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
