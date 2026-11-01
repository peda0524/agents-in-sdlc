---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースとクイックチャットを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤として GitHub と統合されているため、リポジトリ、ブランチ、CI pipeline をすぐに利用できます。分離された workspace で複数の agent を並行して動かし、繰り返し作業を自動化するワークフローに適しています。Node.js のインストールとプロジェクトの準備ができたので、アプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- workspace とテンプレートに含まれるバックログを確認します。
- アプリについて知るためにクイックチャットを試します。

## シナリオ

チームは増え続けるバックログに対応するため AI agent を導入しています。Copilot app では、issue の選択、agent の実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または Pro、Pro+、Business、Enterprise のいずれかを利用してください。Copilot Business または Copilot Enterprise では、管理者が **GitHub Copilot app** policy を有効にしている必要があります。

## GitHub Copilot app をインストールして設定する

1. ブラウザーで [GitHub Copilot app のランディングページ][download-app]を開きます。
2. 使用するプラットフォーム向けの app をダウンロードし、案内に従ってインストールします。Windows、macOS、Linux で利用できます。
3. アプリを開き、**Sign in to GitHub** を選択して認証します。データ所在地を指定する GitHub Enterprise Cloud を使う場合は **Use GitHub Enterprise** を選択し、求められたら `*.ghe.com` の hostname を入力します。GitHub Enterprise Server はサポートされていません。
4. オンボーディングでリポジトリを選択する場合は、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択を後回しにし、オンボーディング完了後に追加することもできます。
5. テーマを選択してオンボーディングを完了します。
6. サイドバーの **Sessions** でリポジトリを確認します。すでに表示されていれば、再度追加する必要はありません。
7. 接続されていない場合は **Sessions** の横にある **+** を選択し、新しい session を作成して **Add GitHub repository** を選びます。前のレッスンで控えた owner と repository name を使います。
8. リポジトリがサイドバーに表示されることを確認します。ここでは `tailspin-toys` などの repository folder の下に個々の session が表示されます。

## ワークスペースを確認する

サイドバーには次の領域があります。

- **New**: Copilot との新しいチャットを開始します。
- **Pull requests**: PR を参照・絞り込み、CI 状態を確認し、レビューします。
- **Issues**: issue を参照・絞り込み、issue から session を開始します。
- **Automations**: schedule またはオンデマンドで実行する agent task を保存します。繰り返し作業に便利ですが、ワークショップでは次のステップとして紹介します。
- **Customize**: MCP server、plugin、skill などを追加します。後で Playwright MCP を設定します。
- **Chats**: branch や workspace を必要としない質問やブレインストーミング向けの会話です。最初の chat を始めると表示されます。表示されない場合は **Sessions** の横にある **+** で chat を始めてください。
- **Sessions**: repository folder と agent session をまとめます。リポジトリ名を選ぶと repository view、session を選ぶと会話が開きます。各 session は独立した workspace で実行されます。

> [!TIP]
> 操作方法が分からない場合や、何かが可能か知りたい場合は Copilot に尋ねてください。

### 用意されたバックログを確認する

1. サイドバーの **Sessions** で Tailspin Toys の repository folder を選択します。個々の session ではなくフォルダーを選び、repository 上部の **Issues** を開きます。
2. issue 番号ではなく、次のタイトルを検索します。

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. issue を選択して詳細を読みます。issue から agent session を開始できます。最初に小さな変更を終えた後、フィルタリング issue から作業します。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが表示されます。ほかのリポジトリの作業項目を見るには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

**Quick chat** を使うと、branch や worktree を作らずに質問やブレインストーミングができます。

1. サイドバーで **Chats** の横にある **+** を選択して新しい chat を開きます。**Chats** がまだ表示されない場合は、**Sessions** の横の **+** を選び、最初の chat を開始します。
2. 次の質問をします。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 回答を読みます。各 session は独自の git worktree で動くため、複数の agent を並行しても変更が競合しないことを確認できます。会話は続けることも、新しい chat を始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールして GitHub にサインインし、リポジトリを追加して workspace とバックログを確認しました。また、クイックチャットを使って簡単な質問をしました。

次は[最初の agent session を開始][next-lesson]し、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app を使い始める][getting-started]
- [GitHub Copilot app で agent session を操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
