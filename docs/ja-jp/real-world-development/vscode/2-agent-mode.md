---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリの変更でも、複数のファイルを更新してテストを実行する必要があります。Copilot Agent Mode は IDE 内でより自律的に動作し、プロジェクトを調査して複数ファイルを更新し、テストを実行して問題を修正します。この演習では Tailspin Toys にフィルタリング機能を追加します。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途です。Agent mode は IDE 内で素早くやり取りしながら作業します。Cloud agent は issue とプルリクエストを通じて、チームの一員のように非同期で作業します。

この演習では、Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して UI とデータ レイヤーにまたがる機能を実装する方法、生成された変更とテストをレビューする方法を学びます。

## シナリオ

ゲーム一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習では publishers helper を追加しました。残りのデータ レイヤー、UI、テストを Copilot Agent Mode で仕上げます。

## Tailspin Toys の Web サイトを実行する

Tailspin Toys は、ボードゲームのクラウドファンディング サイトです。Astro app 1 つで構成され、ページはビルド時に静的 HTML として生成されます。ページ frontmatter から Drizzle ORM 経由でローカル SQLite データベースを参照します。別の backend API や client-side UI framework はありません。再利用可能なデータアクセス helper は `src/lib/` に置き、操作性は標準 DOM API を使う小さな Astro `<script>` で追加します。

変更を始める前に、サイトの現在の動作を確認します。ホームページにはゲームの一覧があり、ゲームを選ぶとタイトル、説明、パブリッシャー、カテゴリなどを含む詳細ページが表示されます。

1. 前の演習後の `main` に戻ったことを確認し、branch を作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

2. 次のコマンドでサイトを起動します。

   ```bash
   npm run dev
   ```

3. ターミナルに表示された `http://localhost:4321` を、Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択してサイトを開きます。Codespace では URL が `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。
4. ホーム ページのゲーム一覧と、ゲームを選択したときに表示される詳細ページを確認します。

> [!NOTE]
> `http://localhost:4321` を開くブラウザー ダイアログが表示された場合は、**x** を選択して閉じます。Codespace のターミナルで localhost のリンクを選ぶと、Codespace の Web サーバーに接続する専用トンネル URL に自動的に転送されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> Copilot Chat を開いて **New Chat** を選択し、新しい会話を始めます。

1. Chat view の agent picker で **Agent** を選択します。

   ![Chat view の agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)
2. モデルの一覧から **Claude Sonnet 4.5** を選択します。
3. 次のプロンプトを送信し、リポジトリの issue を表示して優先順位を付けます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. コマンドの実行を求められたら **Continue** を選択し、issue の一覧を確認します。
5. Copilot がユーザーに最も役立つと考える順に issue を並べたことを確認します。

> [!CAUTION]
> ワークショップで Claude Sonnet 4.5 を使っていますが、特定のモデルを推奨するものではありません。LLM は確率的に動作するため、生成結果が説明と異なる場合があります。これは想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP command を使う場合がありますが、タスクを完了できるはずです。

## Instruction file を確認する

`.github/instructions/ui.instructions.md` を開き、サイトに機能を追加するときの指針を確認します。アーキテクチャの概要、component 設計、テスト可能性、アクセシビリティに関する原則に加え、Astro、Tailwind CSS、Drizzle 向けの個別指示へのリンクがあります。

> [!TIP]
> instruction file から、ほかの instruction file やプロジェクト内ファイルを参照できます。参照パスは instruction file の場所からの相対パスです。これによりルールを再利用し、大きな指示を小さく分け、例やテンプレートを示せます。

## フィルタリング機能を実装する

フィルタリングにはデータ レイヤー (`src/lib/`)、テスト、ゲーム一覧ページの UI を更新する必要があります。

1. 今の会話を続けるか、**New Chat** を選びます。
2. **Add Context**、**Instructions**、**ui** を選択します。

   ![UI instruction file を選択する画面](../../../_images/ex3-select-instructions-file.png)

3. agent picker で **Agent** を選びます。

   ![Chat view の agent picker を示す画面](../../../_images/shared-chat-mode-selector.png)
4. 次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

5. Copilot が関連ファイルを調査し、データ レイヤー、UI、テストを変更してチェックを実行する様子を確認します。

   ![Copilot がプロジェクト内のファイルを調査している画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot がファイルを調査し、編集してテストを実行するには数分かかる場合があります。作業中はエディターに **Keep** と **Undo** が表示されることがあります。すべての変更が終わるまで操作せず、完了後にまとめてレビューしてください。

6. Copilot がテスト実行の許可を求めたら **Continue** を選択します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

7. 作業中にテストが失敗しても、Copilot がコードの作成とテストを反復して問題を修正する様子を確認します。

   ![Copilot Agent Mode の完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

8. Copilot が生成したコードを調べ、潜在的な問題がないか確認します。
9. サイトを開いているブラウザーに戻り、新しい機能を試します。
10. すべての変更と動作をレビューした後、Copilot Chat で **Keep** を選択します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

## まとめと次のステップ

Copilot Agent Mode が UI とデータ レイヤーを更新し、関連ファイルを調査してテストする方法を確認しました。この演習では、次のことを学びました。

- Copilot Agent Mode が UI とデータ レイヤーにまたがる機能を実装する方法。
- Copilot Agent Mode がプロジェクトを調査し、関連ファイルを特定して変更を調整する方法。
- Copilot Agent Mode が生成した変更とテストをマージ前にレビューする方法。

次は[Playwright MCP server で機能をテストします][next-lesson]。

### ボーナス演習 — ページネーションを実装する

ゲーム一覧が増えた場合に備え、Copilot にページネーションの実装を依頼します。既存の instruction file を使うこと、ページあたりの表示件数をユーザーが選べるようにするか固定値にするか決めること、希望する動作を明確に伝えることを検討してください。必要に応じてコンテキストを追加して反復し、修正を依頼します。Copilot と作業する際は、通常このようにやり取りを重ねます。

## リソース

- [Copilot ask、edit、agent mode の役割][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
