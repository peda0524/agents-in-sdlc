---
title: "演習 5 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

Copilot を agent mode で使うと、コードベースの調査、変更の提案、ファイル編集、コマンド実行を自律的に行います。VS Code の Copilot Chat ではツール呼び出しやファイル編集をリアルタイムで確認し、diff をレビューして個別の変更を適用または取り消し、追加のプロンプトで作業を調整できます。

この演習では、アクセシビリティ エージェントの作業を監視し、変更をレビューして適用し、同じセッションにライト モードを追加するよう指示します。

## シナリオ

[Exercise 4][previous-lesson] で選択したアクセシビリティ エージェントはハイ コントラスト モードを実装しています。作業中にデザイン チームからライト モードも追加してほしいと依頼がありました。新しい会話を始めるのではなく、既存のセッションに指示を追加します。

## Copilot の進行状況を監視する

Copilot Chat view では agent が調べたファイル、実行したコマンド、提案した編集を確認できます。各編集は diff と **Keep** / **Undo** の操作とともに表示されます。

1. Exercise 4 で使った Copilot Chat view に戻ります。
2. ファイルの読み込み、編集、実行を求めるターミナル コマンドを確認します。
3. **Keep** で変更を受け入れ、**Undo** で取り消して再試行させます。
4. Chat 上部の **Files changed** で、このセッション中の変更ファイルを確認します。
5. エージェントがハイ コントラスト モードを実装し、完了を報告するまで待ちます。

> [!NOTE]
> ターミナル コマンドなどの実行許可を求められた場合、agent は **Allow** の確認を表示します。内容を確認し、許可してよいコマンドだけを承認してください。

## ライト モードを追加するよう指示する

同じ会話を続けるため、agent は編集した component、スタイル、local storage の使い方などのコンテキストを保持しています。

1. Copilot Chat の入力欄に次の prompt を送信します。

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. エージェントが既存の変更を確認し、ライト モードを追加する様子を確認します。
3. 各 diff をレビューし、**Keep** または **Undo** を選びます。
4. 望まない方向に進んだ場合は入力欄の **Stop** で停止し、補足の prompt を送信します。

> [!TIP]
> 進行中のセッションに指示を追加すると、最初からやり直すより短時間で調整できます。たとえば CSS variable を使って簡潔にする、既存の設定パネルに追加する、両方の toggle にキーボード ショートカットを付けるよう依頼できます。

## 変更内容をレビューする

1. VS Code の **Source Control** view を開き、変更ファイルを確認します。Astro component、スタイル、関連テストなどが含まれます。
2. ファイルを開き、diff を確認します。ARIA 属性、キーボード操作、semantic HTML、local storage への保存などを確認してください。
3. 問題がなければ Source Control panel から変更を stage して commit します。次のレッスンで公開します。

## まとめと次のステップ

エージェントの作業を監視し、個々の編集を受け入れたり取り消したりし、同じセッションに追加の prompt を送って作業を拡張しました。次はアクセシビリティ機能を[プルリクエストとして公開し、変更をレビューします][next-lesson]。

## リソース

- [VS Code の Copilot Chat][copilot-chat-vscode]
- [Agent mode の使用][agent-mode]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
