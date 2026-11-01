---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "フィルタリング、指示、skill の更新、QA profile、テストをレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、instruction の更新、skill の更新、QA profile、テストは同じ branch に保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価の PR は自分でマージしましたが、今回は **Agent Merge** に進行を管理させます。

> [!NOTE]
> 通常は機能、instruction、skill、QA agent を複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質ワークフローを 1 つの会話と branch にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge が PR のマージまでの流れを自動化する方法を学びます。
- 機能全体と検証結果を確認します。
- フィルタリング PR を作成します。
- レビュー後に Agent Merge を有効にし、マージを確認します。

## シナリオ

フィルタリング機能を計画、実装、検証しました。次は残りの PR 作業を自動化しますが、マージを許可する判断は開発者が行います。

## Agent Merge について

**Agent Merge** は PR をマージするまでに必要な残りの作業を自動化します。有効にすると、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要な rebase など、PR をブロックしている問題に取り組みます。リポジトリが許可している場合は、GitHub auto-merge を有効にします。

Agent Merge を使う前に変更をレビューし、マージしてよい状態か判断してください。

## Agent Merge で PR を管理する

1. Codespace に戻ります。
2. `/agent` を入力してエージェント ダイアログを開きます。
3. 一覧から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` を実行して新しい PR を作成します。
5. `/pr agentmerge` を実行して Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーション (CI) を監視し、すべてのチェックが成功した後にマージすることを確認します。
7. Copilot から `PR #14 was squash-merged successfully.` のような成功メッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は必須の承認、branch protection、merge queue、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は理由を確認し、リポジトリで許可されている手順に沿ってレビュー後に手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、PR の処理を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge と、マージまでのライフサイクルを自動化する仕組みを学びました。
- 機能全体と検証結果を確認しました。
- フィルタリング PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次はコンテキスト、モデル、共有、任意の cloud delegation に使える[Copilot CLI のスラッシュ コマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
