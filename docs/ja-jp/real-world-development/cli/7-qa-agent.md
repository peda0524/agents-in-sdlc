---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "`quality-checks` skill、issue の要件、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` skill を使って自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリングを確認しました。次はこれらを、明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、skill、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して調べます。
- QA エージェントを選択し、フィルタリング issue に照らして結果をレビューします。

## シナリオ

Tailspin Toys は PR 作成前に、要件、コード品質、自動チェック、テスト カバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA の手順をまとめて再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、目的、指示、利用可能なツールを Markdown profile で定義する専門化された Copilot です。このレッスンでは `.github/agents/qa.agent.md` に QA role を定義し、Copilot CLI で選択します。

カスタマイズにはそれぞれ役割があります。リポジトリ指示はチーム標準を示し、`quality-checks` skill は再利用可能なチェックをまとめ、Playwright MCP はブラウザー ツールを提供します。QA profile は、それらをどう使って要件を評価し、結果を報告するかを指示します。

QA profile は各機能を要件に照らして確認し、報告を作成します。指示、skill、MCP ツールを置き換えるものではなく、別の会話を始める必要もありません。

## QA profile を作成する

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次の profile を作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## profile を確認する

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を確認します。`description` は必須です。`name` は省略可能ですが、表示名として設定できます。
3. 要件から確認を始め、リポジトリ指示に従い、`quality-checks` skill と Playwright MCP を使うことを確認します。
4. 証拠を示して結果を報告し、実装コードを変更する前に確認を求め、commit や PR を行わないことを確認します。
5. 不足や境界の問題があれば、続ける前に既定のエージェントに修正を依頼します。

## issue に対して QA を実行する

Copilot CLI は会話開始時に project agent を読み込みます。profile を作成した後、同じフィルタリング会話を再開し、QA を選択します。

1. 次のコマンドを入力してエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しく作ったエージェントが利用可能な一覧にまだ表示されない場合でも、上記のコマンドで有効にできます。

2. 次のプロンプトで機能をレビューします。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始したら、完了後にレポートを確認します。

## まとめと次のステップ

再利用可能な専門 role を追加し、その作業結果をレビューしました。具体的には、指示、skill、MCP ツールと連携する方法を確認し、QA profile を作成して調べ、QA エージェントを選択してフィルタリング issue に照らした結果を確認しました。

実装、skill の更新、QA profile、テスト、検証レポートが揃いました。次は結果を[feature PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
