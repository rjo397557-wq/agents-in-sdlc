---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップ アプリケーションで、エージェント主導の開発を 1 つの作業用ワークスペースで実現します。エージェント セッションの並行実行、セッション モードの切り替え、共有 Canvas、GitHub issue とプルリクエストのネイティブな管理機能を備えています。さらに **Agent Merge** を使うと、プルリクエストのリベース、レビュー フィードバック、継続的インテグレーション (CI) の修正、マージまでを進められます。

このワークショップでは、Tailspin Toys で一連のワークフローを体験します。

1. プロジェクトを準備し、アプリをインストールして、リポジトリを接続します。ワークスペースと、あらかじめ用意されたバックログも確認します。
2. 星評価の小さな変更を行い、ブラウザーでレビューして、最初のプルリクエスト (PR) を手動でマージします。
3. フィルター issue から始め、**Plan** モードで方針を決め、**Autopilot** モードで作成して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を確認します。
7. 品質保証 (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の根拠をレビューします。
8. フィルター機能の変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. Canvas が利用できるか確認し、Database Explorer を使います。その後、リポジトリに保存されるトリアージ Canvas を作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替方法を使うか、まとめに進みます。

ワークショップでは、星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能の 2 つの PR を作成します。どちらも更新済みの `main` から始めます。フィルターと品質ワークフローでは、1 つのセッション、worktree、ブランチを共有するため、各ツールを試しながら作業を活用できます。最後の Canvas 演習も同じセッションで行い、PR ワークフローを繰り返すのではなく、共有された作業領域の作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトのコピーを作成します |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認します |
| [2. 星評価を追加する: 手軽な改善][ex2] | 最初の変更 | 既存の評価と null の場合の表示を追加し、最初の PR をマージします |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画し、Autopilot で作成して、Interactive モードでレビューします |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、フィルター機能に適用します |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し実行できるチェック | 既存のスキルを確認し、レポート形式を変えて実行します |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの確認 | Customize から MCP を構成し、フィルターの動作を確認します |
| [7. QA エージェントを作成して使う][ex7] | 要件とカバレッジ | 専門家のプロファイルを作成して選び、最終検証の根拠を収集します |
| [8. 機能 PR を作成してマージする][ex8] | レビューとマージ | フィルター、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使います |
| [9. Canvas を確認して作成する][ex9] | コラボレーション | Database Explorer を使ってから、リポジトリに保存されるトリアージ Canvas を作成してテストします |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加リソースを確認します |

## 前提条件

ワークショップに参加する前に、次のものを用意してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかのプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** のいずれかを実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランをお持ちでない場合、認定を受けた学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使うエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

> [!NOTE]
> Copilot app は Codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトのコピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使う場合、管理者は **GitHub Copilot app** ポリシーを有効にする必要があります。これは Copilot CLI ポリシーとは別の設定です。

## はじめに

**[前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
