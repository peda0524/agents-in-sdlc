---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザー エクスペリエンスと機能を改善する 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期に自律して動きますが、作業を監視することも重要です。

GitHub.com の [agents ページ][agents-page]などを使って、cloud agent に割り当てたタスクを管理できます。ここでは、開いているプルリクエストがあるタスクを確認し、実行中のセッションを調べて、進行中の作業に指示を出せます。

このレッスンでは、次のことを行います。

- agents ページで cloud agent のタスクを監視します。
- 実行中のセッションに追加の機能を依頼します。

前の演習で依頼した作業は、ドキュメントの追加、ゲームを変更する API の生成、サイトのハイ コントラスト モードの追加です。

## シナリオ

ハイ コントラスト モードを作成している最中に、チームはライト モードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含められます。エージェントに作業を続けてもらい、両方のモードを追加するよう指示します。

## Copilot cloud agent のタスクを確認する

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン ペインと左ペインのタスク一覧で、ドキュメントの更新、ゲームの API 生成、ハイ コントラスト モードの追加など、割り当てたタスクを確認します。
3. 実行中のタスクを選択し、コードのチェックアウト、環境作成、MCP サーバーの設定など、Copilot が実行した手順を確認します。表示される手順は、状態や作業方法によって異なります。
4. 右側のプルリクエスト ペインで、PR と変更ファイルも確認します。

## cloud agent に指示を出す

ハイ コントラスト モードの作業に、ライト モードも追加するよう依頼します。

1. ハイ コントラスト モードのセッションを選択します。タイトルはエージェントが付けた名前や進捗状況によって異なる場合があります。

   ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分待ちます。
   Copilot が作業を開始すると、セッションに進行状況が表示されます。
3. **Steer active session while Copilot is working** ダイアログで次のプロンプトを送信します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

4. <kbd>Enter</kbd> を押して送信し、Copilot が依頼を認識して作業に取り込むことを確認します。

![agents ページで cloud agent の作業中セッションを案内するダイアログを示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

## Copilot に作業を続けてもらう

Copilot は、メッセージを送信した時点で作業中の手順を完了してから、新しい依頼を取り込みます。完了まで数分かかることがあります。その間に、これまでに学んだことを振り返りましょう。

## まとめと次のステップ

Copilot agents ページは cloud agent のタスクを監視・案内する中心的な場所です。進行中と完了済みのタスクを追跡し、作業内容を確認して、進行中のセッションに追加の機能を依頼する方法を学びました。

このレッスンでは、次のことを行いました。

- agents ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

次は[Copilot のプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
