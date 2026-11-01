---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

多くの組織では、未解決のセキュリティ問題、更新が必要なレガシー コード、バックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行し、完了すると人がレビューできるドラフト PR を作成します。時間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

このレッスンでは、次のことを行います。

- コード生成環境をカスタマイズします。
- 安全に操作する方法を確認します。
- issue の範囲を明確にする重要性を学びます。
- issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys では、アプリケーションの公開関数に TSDoc doc comment を追加する作業と、ゲーム詳細ページに同じカテゴリの関連ゲームを表示する作業が必要です。いずれも cloud agent に非同期で任せることで、別の作業を進めながら結果を後でレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者のようにバックグラウンドで作業できます。たとえば [GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)と、作業を追跡するドラフト PR を作成し、環境を準備してタスクを開始します。作業中も完了後もセッションを確認でき、レビューの準備ができるとプルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot を使っても、考える作業を省略できるわけではありません。タスクを適切な範囲に設定し、AI ペア プログラマーとして段階的に試し、結果を確認してください。生成 AI を使っても、ソフトウェア開発の基本は変わりません。[cloud agent のタスク設定に関するベスト プラクティス][cloud-agent-best-practices]を確認してください。

## このリポジトリのカスタム指示

前の演習ではカスタム指示を追加しました。cloud agent に作業を割り当てる前に、次のファイルを GitHub の Web UI または codespace で確認してください。

- `.github/copilot-instructions.md` の **Code standards** セクション。TypeScript の規約、TSDoc doc comment、ファイル コメント ヘッダーを確認します。
- `.github/instructions/unit-tests.instructions.md` の `applyTo` frontmatter。`**/*.test.ts` に一致するファイルに指示が適用されます。

後で `Code lacks documentation` issue の PR をレビューするとき、指示に沿った TSDoc、TypeScript 規約、コメント ヘッダーを確認します。

## cloud agent の開発環境を準備する

cloud agent は GitHub Actions の環境で作業します。`.github/workflows/copilot-setup-steps.yml` に専用のセットアップ ワークフローを置くと、必要な開発ツールや依存関係を準備できます。このラボでは、Node.js、プロジェクト依存関係、エンドツーエンド テスト用のブラウザー、SQLite データベースを用意するワークフローが事前に設定されています。

ワークフローには `copilot-setup-steps` というジョブがあり、Copilot が PR に取り組む前に実行されます。`workflow_dispatch` トリガーにより、Copilot の実行を待たずにリポジトリの **Actions** タブから手動でテストできます。セットアップ ワークフローでは、必要最小限の権限だけを付与してください。

環境のセットアップに使われるワークフローは、通常の GitHub Actions workflow と同じ形式です。テンプレートには、checkout、Node.js と npm 依存関係、Playwright のブラウザー、移行・seed 済み SQLite データベースを準備する次の設定が含まれています。

```yaml
name: "Copilot Setup Steps"

# repository の "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot の操作には独自の token が付与されます。
    permissions:
      # 依存関係のインストールなどでセットアップ手順中に repository を clone する場合は、`contents: read` 権限が必要です。clone しない場合は、手順の完了後に Copilot が自動で clone します。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / app のセットアップ - Node.js (現在、app 全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # build とテストに必要なデータを用意するため、ローカル SQLite データベースを migrate して seed します。
      - name: Set up the database
        run: npm run db:setup
```

この設定では、セットアップに必要な範囲に権限を制限します。セットアップで repository を clone する場合は `contents: read` が必要です。clone しない場合は、手順の完了後に Copilot が自動で clone します。

## ドキュメントを追加する issue を作成する

1. github.com でリポジトリを開き、**Issues** タブ、**New issue**、**Blank issue** の順に選択します。
2. **Title** に `Code lacks documentation` と入力します。
3. **Description** に次のテキストを入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

4. **Create** を選択して issue を作成します。
5. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

6. **Assign** を選択します。

   ![Copilot への割り当て内容を示す画面](../../../_images/ex4-assign-copilot-details.png)

7. **Pull Requests** タブを開き、`[WIP]: Code lacks documentation` のようなタイトルの PR を開きます。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
8. 数分後に PR に Copilot の todo list が表示されたら、実行予定の作業を確認します。

> [!NOTE]
> todo list が表示されるまで数分かかることがあります。Copilot は先に環境を作成し、前述の workflow を実行してからプロジェクトを分析し、作業計画を立てます。

9. PR のタイムラインを下へスクロールし、Copilot が issue の作業を開始したことを確認します。
10. **View session** を選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示が反映されない場合は、ブラウザーを更新してください。

11. ライブ セッションをスクロールし、Copilot がコードと状態を調査し、計画を立てて、コードを作成する様子を確認します。

この作業には数分かかることがあります。cloud agent の利点は非同期で作業できることです。別の issue も割り当て、その間にアプリの機能開発を進めます。

## 関連ゲーム機能の issue を作成する

1. リポジトリの **Issues** タブから **New issue**、**Blank issue** の順に選択します。
2. **Title** に `Show related games on the game details page` と入力します。
3. **Description** に次の要件を記述します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

4. **Create** を選択して issue を作成します。
5. 右側の **Assign to Copilot** を選択して割り当てダイアログを開きます。

   ![issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

6. **Assign** を選択します。

   ![issue の最初のコメントに Copilot の作業中を示す目の絵文字が表示された画面](../../../_images/ex4-issue-eyes-emoji.png)

issue の最初のコメントに 👀 が表示されたら、Copilot が作業を開始したことを示します。

Copilot は SWE と同じように作業を進めます。常に監視する必要はなく、完了後にレビューできます。カスタムエージェントの作成と使用に進みましょう。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] に issue を割り当て、技術的負債への対応、新機能の作成、フレームワーク間のコード移行などを非同期に行う方法を確認しました。開発環境の準備、セキュリティ、issue の範囲、作業の割り当てについて学びました。

このレッスンでは、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 安全に操作する方法を確認しました。
- issue の範囲を明確にする重要性を学びました。
- issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、[カスタムエージェントの作成と使用][next-lesson]に進みます。cloud agent は [MCP サーバー][cloud-agent-mcp]と、前の演習で追加したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
