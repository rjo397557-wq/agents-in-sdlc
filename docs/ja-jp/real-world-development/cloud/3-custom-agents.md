---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせて、専用の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を Copilot に提供できます。チームの専門知識を再利用できるエージェントとしてまとめることもできます。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティ エージェント、安全なコーディングを確認するセキュリティ エージェント、テスト パターンを統一するテスト エージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記述する Markdown プロンプトが必要です。

### カスタムエージェントとエージェント スキルの違い

カスタムエージェントと[エージェント スキル][agent-skills-concept]には、役割が重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業の方法を指示します。最も分かりやすい違いは、**カスタムエージェントは作業者、スキルはツール**だという点です。

カスタムエージェントは独自のコンテキスト ウィンドウを持ち、作業の一部としてスキルやほかのエージェントを使って調整できます。このラボでは、アクセシビリティのカスタムエージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一環として、プルリクエスト ワークフローやテスト実行・管理のスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの使い方と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタムエージェントを使って、次のことを学びます。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラスト モードを実装するよう依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って、この機能を生成します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、すでに用意されています。内容を確認し、Copilot をどのようにガイドするかを理解しましょう。

Codespace に戻り、アクセシビリティ カスタムエージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が説明されています。
   - アクセシブルな Web サイトのコードを生成するときの主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき間違い。

## issue を作成して割り当てる

Mission control は、環境内のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当てたり、タスクを監視したり、実行中のセッションに方向付けや追加のガイダンスを与えたりできます。まず、Copilot にハイコントラスト モードの作成を依頼します。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の文を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てのダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントの割り当て画面。Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野向けの GitHub Copilot 専用 AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot が適切に作業するためのガイドを提供できます。

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントにタスクを割り当てる方法。

Copilot がハイコントラスト モードを実装している間に、次は [mission control からエージェント セッションを監視し、方向付けます][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
