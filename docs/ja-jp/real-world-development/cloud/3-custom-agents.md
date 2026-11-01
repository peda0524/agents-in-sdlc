---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、専門知識、ベスト プラクティス、指示を与えます。たとえば、[WCAG][wcag] に沿ったアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、テスト規約を守るテスト エージェントをチームで再利用できます。

カスタムエージェントはプロジェクト内の `.github/agents`、またはユーザーの `~/.copilot/agents` に Markdown ファイルとして定義します。ファイルには少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門性、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントと agent skill の違い

[Agent skill][agent-skills-concept]とカスタムエージェントには共通点があります。どちらも主に Markdown ファイルで定義し、AI が作業を行う方法を説明します。カスタムエージェントは作業者、skill は作業者が使うツールと考えると分かりやすいでしょう。

カスタムエージェントは独自のコンテキストを持ち、skill や別のエージェントを統括できます。このラボのアクセシビリティ エージェントはサイトを確認して更新し、必要に応じて PR ワークフローやテスト実行用の skill を使います。

この演習では、アクセシビリティ エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローやテストを実行・管理する skill を呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。利用環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず誰もが利用できるクラウドファンディング プラットフォームを目指しています。現在のダーク テーマは文字と背景のコントラストが不十分で、読みにくいというフィードバックがありました。そこで、ユーザーが切り替えられるハイ コントラスト モードを追加します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタムエージェントを確認します。
- Copilot cloud agent でアクセシビリティ エージェントを使います。

## アクセシビリティ カスタムエージェントを確認する

アクセシビリティ エージェントはすでに用意されています。codespace に戻り、`.github/agents/accessibility.md` を開きます。

1. YAML frontmatter の `name` と `description` を確認します。
2. Web サイトのアクセシビリティ、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題に関するセクションを読みます。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

## issue を作成してエージェントに割り当てる

1. リポジトリを開き、**Issues** タブ、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Add high contrast mode to website` と入力します。
3. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

4. **Create** を選び、issue を作成します。
5. 右側の **Assign to Copilot** を選択し、割り当てダイアログを開きます。
6. 一覧から **Accessibility agent** を選択します。

![カスタムエージェントと Accessibility が選択された cloud agent 割り当て画面のスクリーンショット](../../../_images/ex5-select-custom-agent.png)

7. **Assign** を選択します。Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

[カスタムエージェント][custom-agents]は、特定のタスクや分野に特化した Copilot です。チームの専門知識や標準を再利用できる形にし、Copilot の作業を案内できます。

この演習では、次のことを行いました。

- カスタムエージェントの定義方法を確認しました。
- カスタムエージェントにタスクを割り当てました。

Copilot がハイ コントラスト モードを実装している間に、[agents ページでセッションを監視して指示を出します][next-lesson]。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使う準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使う準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
