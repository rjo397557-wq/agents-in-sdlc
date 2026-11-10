---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善や機能の追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチームが試せる関連ゲーム機能を作成し、ハイコントラストとライトモードの切り替えなど、アクセシビリティ機能を実装するよう依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードを必ずレビューし、通常の DevOps プロセスに沿って作業します。チーム全体にレビューを依頼する前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期に、監督なしでタスクを実行するため、安全性を維持するためのセキュリティ制約があります。

- Copilot はリポジトリに対する読み取りアクセス権を持ち、コードを書き込めるのは作業に使うブランチ**のみ**です。
- cloud agent は GitHub Actions で実行され、作業用の一時的な環境を別に作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使って確認します。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示されることがあります。Copilot は外部 MCP サーバーへの呼び出しなど、外部リソースへのアクセスが既定で制限されているため、これは**想定どおり**です。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が含まれます。その場合は、Copilot が作業を終えるまで待ちます。数分かかることがあるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメント コメントなど、新しく追加されたドキュメントを含むコードを確認します。実際の変更内容は状況によって異なります。

    変更を確認しながら、TSDoc のドキュメント コメント、TypeScript の規約、コメント ヘッダーを確認します。これらは演習 2 の最初に確認したカスタム指示ファイルから反映されています。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

Copilot とプルリクエストでやり取りするのは、一方通行ではありません。チームのほかのメンバーと同じように、プルリクエストのコメントやコードのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ずしも適切な依頼文を提示できるとは限りません。たとえば、次のような変更を Copilot に依頼できます。

- 各コード ファイルの先頭に、ファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. **@copilot** をユーザーと同じようにメンションして、生成されたドキュメントに変更を依頼するコメントを追加します。上記の例か、コードベースに追加したいドキュメントのアイデアを使ってください。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻るリンク](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

満足するまで繰り返し作業できます。準備ができたら、PR をドラフトからレビュー可能な状態に変更し、`main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲームの詳細ページに関連ゲームを表示する issue について、Copilot が作成した PR を確認しましょう。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローが承認待ちになっていることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
9. **任意:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前の作業と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメント ヘッダーや TSDoc のドキュメント コメントを追加したりできます（この issue はカスタム指示を変更**する前**に割り当てたことを思い出してください）。前と同じように **Conversation** タブにコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタムエージェントを使って実装したアクセシビリティ機能をレビューします。この PR には、演習 3 で依頼したハイコントラスト モードと、演習 4 の mission control から追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** フラグが表示されます。その場合は、Copilot が作業を終えるまで数分待ちます。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認するときは、特に次の点に注意します。
   - モード切り替えのトグル UI コンポーネント。
   - ユーザー設定を保存するためのローカル ストレージの使用。
   - ハイコントラスト モードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）。
   - モードの切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェック欄にワークフローがキューに入ったことが表示されます。問題がなければ、単一 Astro アプリのプロジェクト チェックが成功します。完了まで数分かかることがあります。
10. **任意:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、次のコマンドを実行します（`<branch-name>` を Copilot が作成したブランチ名に置き換えます）。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリケーションを起動し、ブラウザーでハイコントラストとライトモードのトグルをテストします。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ カスタムエージェントがアクセシビリティのベスト プラクティスに沿って Copilot の実装をガイドしたことを確認します。アクセシビリティの問題や改善点があれば、前の PR と同じように **@copilot** をコメントでメンションして更新を依頼できます。

## 任意の演習 — さらに作業を委任する

cloud agent は、実際のバックログのタスクを任せて、別の作業に集中するときに役立ちます。リポジトリにさらに issue を登録し、Copilot に割り当ててみましょう。次のようなタスクが考えられます。

- ゲームの詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。各レッスンで、次のことを行いました。

- **このリポジトリにあるカスタム指示ファイルを確認し**、cloud agent の出力に反映されることを後で確認できるようにしました。
- **issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ向けの**カスタムエージェントを作成して使用し**、ハイコントラストとライトモードのトグルを追加しました。
- **Copilot agents ページを mission control として使い**、実行中のアクセシビリティ セッションを監視して方向付けました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` をメンションして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、次のハーネスでも今回の経験を発展させられます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat の agent mode と MCP 統合を体験します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent への連携を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでも、次のような作業を試してみましょう。

- リファクタリングやテスト カバレッジの issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
