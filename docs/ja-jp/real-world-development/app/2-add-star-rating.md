---
title: "レッスン 2 - 星評価の追加: 小さな成果"
description: "GitHub Copilot app で最初の agent session を開始し、ゲーム カードに小さな変更を加え、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

workspace を確認し、クイックチャットを試しました。次は **agent session** を開始し、プロジェクトに最初の変更を加えます。ゲーム データには星評価がありますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- agent session を開始し、session の構成を確認します。
- エージェントに小さく範囲を絞った変更を依頼します。
- workspace の diff view で変更をレビューします。
- アプリをローカルで実行し、ブラウザーで確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。ホーム ページのカードには、タイトル、カテゴリ、パブリッシャー、説明しかありません。最初の session では既存の評価をカードに表示します。

## Session の構成

**Session** は、独立した workspace で実行されるエージェントとの会話です。各 session には専用の **git worktree と branch** があり、一方で機能を追加しながら、別の session でバグを修正するなど、変更を競合させずに複数の作業を並行できます。session はサイドバーでリポジトリごとにグループ化されます。

session では、エージェントとの **conversation**、ファイルを調査・編集する **tool activity**、差分を含む変更ファイル一覧を確認できます。

## Session を開始して変更を依頼する

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択して session を開始します。
3. repository として `tailspin-toys` を選びます。
4. プロンプト欄の下で **new working tree** と **Interactive** mode を選択し、次の prompt を送信します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot app は新しい worktree を作成し、必要なファイルを調査してコードを作成します。

## 差分をレビューする

1. アプリ右上の **Toggle review panel** を選び、開いた panel で **Changes** を選択します。

   ![Create PR ボタンの右側にある Toggle review panel ボタンを示す GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. `GameCard.astro` に、評価がある場合の表示と `starRating` が `null` の場合の `No rating yet` 表示が追加されていることを確認します。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は確率的に動作するため、生成コードは例と異なる場合があります。比較的似た実装になるはずです。

## 変更を確認する

ブラウザーを開く前に自動チェックの結果を確認し、数値の `starRating` と `null` の代替表示の両方をテストしていることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

> [!TIP]
> Canvas は Copilot app 内で使えるインタラクティブな widget です。後の演習ではカスタム Canvas を調べます。ここでは組み込みのブラウザー Canvas を使います。

1. 次の prompt でアプリを起動し、ブラウザー Canvas で開くよう依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. アプリ内でブラウザーが開いたら、評価済みのゲーム カードに 5 段階評価が表示されることを確認します。
3. 次の prompt で開発サーバーを停止し、ブラウザー Canvas を閉じます。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

1. 右上の **Create PR** を選択します。
2. 必要に応じて **Sign in with your browser** を選び、認証します。
3. リポジトリ policy により PR 作成前に関連 issue が必要だと Copilot が伝えた場合は、その手順に従うよう確認します。
4. Copilot が PR を作成したら、チャットの上にある **PR** bubble を選択し、review pane で確認します。
5. 準備ができたら **Ready to merge** を選び、ダイアログで **Merge pull request** を選択します。

## まとめと次のステップ

最初の agent session を開始し、ゲーム カードに評価を追加しました。差分をレビューし、アプリをローカルで実行して確認し、PR 1 のチェックをレビューして明示的にマージしました。

次は[フィルタリング issue から始め、Plan と Autopilot モードを使って][next-lesson]より大きな機能を実装します。

## リソース

- [GitHub Copilot app で agent session を操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして設定する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
