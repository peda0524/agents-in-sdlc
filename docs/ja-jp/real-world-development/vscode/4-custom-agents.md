---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]は、開発ワークフローの特定のタスクや分野に特化した AI アシスタントです。リポジトリの `.github/agents` にある Markdown ファイルで定義し、専門知識、ベスト プラクティス、コード パターンを指定します。たとえば [WCAG][wcag] に沿ったアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、テスト規約を守るテスト エージェントを作成できます。

各ファイルには `name` と `description` を含む YAML frontmatter と、エージェントの動作を説明する Markdown prompt を記述します。プロジェクトの `.github/agents` またはグローバルな `~/.copilot/agents` に配置できます。

### カスタムエージェントと agent skill の違い

[Agent skill][agent-skills-concept]とカスタムエージェントはどちらも Markdown で定義します。カスタムエージェントは作業者、skill は作業者が使うツールと考えると分かりやすいでしょう。カスタムエージェントは独自のコンテキストを持ち、skill やほかの agent を統括できます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるサイトを目指しています。現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。切り替え可能なハイ コントラスト モードを追加するため、アクセシビリティ エージェントを使います。

この演習では、既存のアクセシビリティ エージェントをレビューし、Copilot Chat でハイ コントラスト モードの実装に使います。

## アクセシビリティ エージェントを確認する

Codespace に戻り、アクセシビリティ作業用の branch を `main` から作成します。Exercise 3 のフィルタリング PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

`.github/agents/accessibility.md` を開き、`name` と `description` を含む YAML frontmatter を確認します。アクセシブルなサイトの生成責務、ベスト プラクティス、HTML・CSS・JavaScript の例、よくある問題のセクションも確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat view では、`.github/agents` に定義したカスタムエージェントを agent picker から選べます。

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat を開き、**New Chat** を選択して新しい会話を始めます。

1. Chat view の agent picker で **Agent** を選択します。
2. agent picker を開き、**Accessibility agent** を選択します。
3. 次のプロンプトを送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

Copilot Chat がコードベースを調査し、変更を提案して適用します。変更はファイル パスと diff とともに表示されます。作業中は **Files changed** の一覧で変更されたファイルを確認できます。

> [!NOTE]
> 変更には数分かかる場合があります。Copilot は Astro component、スタイル、関連テストなどを更新します。次の演習では進行中の作業をレビューし、指示を追加します。

## まとめと次のステップ

カスタムエージェントを使うと、特定のタスクや分野に特化した Copilot を作成し、チームの専門知識や標準を再利用できます。この演習では、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- Copilot Chat の agent mode でカスタムエージェントを使用しました。

次は[エージェントの進行状況を監視して追加の指示を出します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP で機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
