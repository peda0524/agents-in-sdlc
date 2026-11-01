---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して、用意されたフィルタリング issue を見つけました。
- 星評価を追加し、転送ブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリング issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを案内し、`quality-checks` skill をカスタマイズしてユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルタリング機能を確認しました。
- quality assurance (QA) カスタムエージェントを作成・選択し、要件、カバレッジ、skill の結果、ブラウザーでの証拠を評価しました。
- フィルタリングの変更全体をレビューし、Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意の cloud delegation に使う slash command を確認しました。

## 実装したもの

ワークショップでは、更新済みの `main` からそれぞれ作成した 2 つの PR を完成させました。

1. **星評価**: 既存の `starRating` と未評価状態をゲーム カードに表示しました。
2. **フィルタリングと品質ワークフロー**: フィルタリングを実装し、instruction を更新して機能に適用し、`quality-checks` の報告形式を変更し、QA profile とテストを追加しました。

ワークショップを簡潔にするため、フィルタリングの計画から PR 作成まで同じ会話と branch を使い、1 つの PR にまとめました。

## 異なる方法で検証する

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー確認を組み合わせました。`quality-checks` skill でユニット テスト、lint、型チェックを実行し、設定した形式で結果を報告しました。QA はこれらの結果、要件、テスト カバレッジをまとめて確認します。

追加したテストで実際の不足を解消できていることを確認します。新しいテストが不要と判断される QA 結果もあり得ます。変更後はコードと証拠を確認し、影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業の質に影響します。このワークショップでは、instruction を更新し、skill をカスタマイズし、QA profile を作り、MCP server を設定しました。instruction は標準を設定し、skill は反復可能な作業を説明し、カスタムエージェントは専門 role を定義し、MCP server は外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った **mode と model** を選びます。実装前に方針を考えるには **Plan**、範囲を絞った変更を対話しながら進めるには **Interactive**、明確に定義したタスクには **Autopilot** を使います。日常的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらいコンテキストも重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が変わります。

## さらに試す

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

ツールの上達には継続して使うことが大切です。本番コード、趣味のコード、作りたいと思っていた小さなアプリなどで試し、学んだことをチームと共有してください。追加の情報はドキュメントで確認できます。

ターミナルから Tailspin Toys を拡張する場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境を比較する場合は、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
