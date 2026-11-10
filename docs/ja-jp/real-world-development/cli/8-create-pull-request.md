---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが、1 つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントをいくつかの PR に分けます。このワークショップでは、作業を簡略化するため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチで進め、1 つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルター PR を作成します。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

フィルター機能の作成中、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの承認権限は開発者に残したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている問題（失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど）に対処し、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができます。ただし、有効にする前に作業をレビューし、準備ができていることを確認してください。

## Agent Merge で PR を管理する

すべてのコードを作成したので、まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. Copilot CLI で `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って、新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. `PR #14 was squash-merged successfully.` のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージ キュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリが許可している場合はレビュー済みの PR を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルター PR を作成しました。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[Copilot CLI のスラッシュコマンド][next-lesson]を使い、コンテキスト、モデル、共有、任意のクラウド委任を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
