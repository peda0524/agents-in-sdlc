---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Tailspin Toys で 2 つの変更を構築、検証、公開しながら、Copilot CLI のモード、カスタマイズ、MCP ツール、プルリクエスト自動化を学びます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** は、ターミナルで GitHub Copilot をエージェント型のコーディング アシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンド ラインから行えるため、グラフィカル エディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って進めます。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして使い方を確認します。
2. 星評価の小さな変更を行い、ブラウザーでレビューして、最初の pull request (PR) を手動でマージします。
3. フィルタリング issue から始め、Plan モードでアプローチを定義し、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルタリング作業に適用します。
5. 既存の `quality-checks` skill をカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) server を追加し、ブラウザーでフィルタリングを確認します。
7. quality assurance (QA) カスタムエージェントを作成し、要件、カバレッジ、検証の証拠をレビューします。
8. フィルタリングの変更全体をレビューし、Agent Merge を使って機能 PR をマージします。
9. コンテキスト、モデル、共有、cloud delegation に使う便利な slash command を確認します。

ワークショップでは、星評価と、指示・skill・QA profile・テストの更新を含むフィルタリングの 2 つの PR を作成します。各 PR は更新済みの `main` から始めます。フィルタリングと品質のワークフローは、会話と branch を 1 つ共有するため、ツールを学びながら作業を積み重ねられます。また、[GitHub Copilot CLI と Foundry を使う全 3 モジュールのオプション シリーズ][foundry]では、モデルを準備し、ホスト型エージェントを構築してデプロイし、サイトに接続できます。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証する |
| [2. 星評価の追加][ex2] | 最初の変更 | ゲーム カードに評価を表示し、最初の PR をマージする |
| [3. エージェント モード: Plan と Autopilot][ex3] | エージェント モード | issue から機能を計画・実装し、レビューする |
| [4. カスタム指示][ex4] | コンテキスト | 指示を確認・更新し、フィルタリング機能に適用する |
| [5. quality-checks skill のカスタマイズと使用][ex5] | 反復可能なチェック | 既存の skill を調べ、報告形式を変更して実行する |
| [6. Playwright MCP による機能の検証][ex6] | 外部ツール | Playwright MCP server でブラウザー上の機能を確認する |
| [7. QA エージェント][ex7] | 要件とカバレッジ | QA profile を作成し、最終検証の証拠を集める |
| [8. 機能 PR の作成とマージ][ex8] | レビューとマージ | 機能をレビューし、Agent Merge で PR をマージする |
| [9. CLI の便利な機能][ex9] | CLI 機能 | コンテキスト、モデル、共有、任意の cloud delegation を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | 重要な概念と次のステップを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいた Backer Concierge を構築してデプロイし、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンド ライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、code review、Copilot CLI の機能が含まれているため、すべての harness を完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
