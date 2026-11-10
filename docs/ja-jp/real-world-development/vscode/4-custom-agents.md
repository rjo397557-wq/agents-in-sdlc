---
title: "演習 4 - カスタムエージェント"
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

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、すべてのユーザーがクラウドファンディング プラットフォームを利用できるようにしたいと考えています。最近のユーザー フィードバックでは、テキストと背景色のコントラストが十分でないため、現在のダークテーマが読みにくいという意見がありました。このアクセシビリティの問題に対応するため、デザインチームはユーザーが切り替えられるハイコントラスト モードを実装するよう依頼しました。

アクセシビリティは重要なため、できるだけ早く実装する必要があります。カスタムエージェントを使って、この機能を生成します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、ハイコントラスト モードを実装します。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、すでに用意されています。内容を確認し、Copilot をどのようにガイドするかを理解しましょう。

まず Codespace に戻り、アクセシビリティ機能の作業用に `main` から新しいブランチを作成します（演習 3 のフィルター PR とは別に作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。次の内容が説明されています。
   - アクセシブルな Web サイトのコードを生成するときの主な役割。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題と避けるべき間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェント ドロップダウンには、`.github/agents` で定義されたすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに応じたチャット セッションになります。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. Chat ビューのエージェント ドロップダウンから、**Agent** を選択します（まだ選択されていない場合）。

   ![Chat ビューで agent picker を選択する画面](../../../_images/shared-chat-mode-selector.png)

2. チャット ビュー下部にあるエージェント ドロップダウンを選択します。現在のエージェントが表示されています。既定では **default** です。
3. 使用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. アクセシビリティ エージェントに次のプロンプトを送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始し、コードベースを調べて、変更を提案し、プロジェクト ファイルを編集します。編集はチャット内にファイル パスと差分とともに表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、エージェントが変更した作業ファイルがわかります。

> [!NOTE]
> この作業には数分かかることがあります。Copilot はリポジトリに実際の変更を行い、Astro コンポーネント、CSS、関連するテストなどの既存ファイルを編集します。

次の演習では、作業中の変更をレビューし、追加の指示を送ります。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野向けの GitHub Copilot 専用 AI アシスタントです。カスタムエージェントを使うと、チームの専門知識や標準を再利用できる形にまとめ、Copilot が適切に作業するためのガイドを提供できます。

この演習では、次のことを学びました。

- カスタムエージェントの定義方法。
- Copilot Chat の agent mode でカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付けます][next-lesson]。変更を確認しながら、同じセッションにライトモードのトグルを追加するよう依頼します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
