---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業を行いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、登録済みのフィルター issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューし、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター issue から始め、Plan モードで方針を決め、Autopilot モードで作成して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選び、要件、テスト カバレッジ、スキルの結果、ブラウザーの根拠を評価しました。
- フィルター機能の変更全体をレビューし、フィルター PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成して届けたもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を完成させます。

1. **星評価:** 既存の `starRating` と未評価状態の表示をゲーム カードに追加します。
2. **フィルターと品質ワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルターの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由の Copilot によるブラウザー操作を使ってコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果に要件とテスト カバレッジのレビューをまとめてから PR を作成しました。

追加したテストで、実際に不足していた箇所を補います。新しいテストを必要としない QA 実行が正しい場合もあります。マージを許可する前にコードと根拠をレビューし、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールによって、作業の結果が変わります。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。ビルド前に方針を検討するには **Plan**、目的を絞った変更に関与し続けるには **Interactive**、範囲の明確なタスクには **Autopilot** を使います。日常的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャだけでなく、コンテキストも重要です。何を、なぜ、どのように作成したいのかを明確に伝えると、出力が大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI には、次のような便利な機能もあります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。実際のコード、趣味のコード、長年作りたいと思いながら手を付けられていない小さなアプリに活用しましょう。学んだことをチームで共有し、チームのメンバーから学びましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
