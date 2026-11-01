---
title: "演習 3 - Playwright MCP server で機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

Agent mode でフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。**Playwright MCP server** を接続し、Copilot に実際のブラウザーを操作させて機能をテストしてから、branch を公開して PR を作成します。

この演習では、Model Context Protocol (MCP) の概要、MCP server が Copilot を拡張する方法、Playwright MCP server の追加、ブラウザー テスト、branch の公開と PR 作成を行います。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI agent が外部ツールやサービスと通信するための標準です。MCP server は、既存の API や npm package などのローカル ツールと agent の間の通信を管理します。MCP を使うと、agent は resources から最新情報にアクセスし、tools を使ってユーザーに代わり操作できます。server ごとに利用可能な tools と resources が異なります。

![agent mode と context、LLM、MCP server や VS Code extension が提供するツールの連携を示す図](../../../_images/ex1-mcp-diagram.png)

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)** は GitHub repository、issue、PR などを管理する API を提供します。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)** はブラウザー自動化を提供します。

GitHub は MCP server を見つけるための [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> MCP server はほかの依存関係と同様に扱ってください。利用前にソース コードを慎重にレビューし、公開元を確認し、セキュリティへの影響を検討します。信頼できる MCP server のみを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP 設定を確認する

`.vscode/mcp.json` は VS Code workspace で利用できる MCP server を設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. すでに設定されている `github` server を確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この設定はテンプレートに含まれるため、前の演習で Copilot が issue を参照できました。`github` server はローカルにインストールせず、HTTP 経由でリモート GitHub MCP server に接続します。Copilot がその tool を初めて使うときに GitHub 認証を求められます。

## Playwright MCP server を追加する

1. `.vscode/mcp.json` の `github` と並べて `playwright` entry を追加します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

Playwright はローカル server で、VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` は画面のない codespace で必要です。Tailspin Toys は E2E テストに Playwright を使うため、ブラウザーはすでにインストールされていることが多いです。Copilot が見つからないと報告した場合は、`npx playwright install chromium` を実行して再試行します。

## Server を起動して信頼する

VS Code は MCP server を必要に応じて起動します。Copilot が server の tool を初めて必要としたとき、VS Code は server を起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` entry の上に表示される **Start** を選び、server を起動して設定を確認します。
2. VS Code から server を信頼するか尋ねられたら、設定を確認してから信頼を選択します。

手動で起動する必要はありません。ここで起動しない場合は、次の手順で Copilot が tool を使うときに VS Code が起動を促します。

## フィルタリング機能をテストする

Playwright MCP server はページの screenshot ではなく[アクセシビリティ ツリー][playwright-mcp-server]を使います。agent はボタン、リンク、リスト項目などの構造化された要素を認識します。

Copilot はブラウザーでページを開き、移動し、フィルターを適用し、結果を読み取って観察内容を要約できます。会話を離れずに動作を確認できます。アクセシビリティ ツリー上のラベル付き要素を使って検証するため、機能確認とあわせて簡単なアクセシビリティ確認にもなります。

アプリが起動している状態で、次のプロンプトを Copilot に送信します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP server を通じてブラウザーを起動し、各手順を実行して結果を返します。issue の受け入れ条件に照らして報告を確認し、問題があれば追加の質問をするか、PR 作成前に修正を依頼します。

> [!NOTE]
> テスト時には `http://localhost:4321` で app が起動している必要があります。開発サーバーを停止している場合は、prompt を送る前に再起動します。Playwright MCP を初めて使うときにブラウザーをダウンロードする場合があります。見つからないと報告されたら `npx playwright install chromium` を実行して再試行します。

## branch を公開して PR を作成する

1. Codespace の **Source Control** panel を開き、Copilot が作成した変更をレビューします。
2. **+** アイコンを選んで変更を stage します。
3. **Sparkle** ボタンで commit message を生成します。

   ![変更内容を示す Source Control panel](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選び、branch を repository に push します。
5. Copilot Chat panel を開き、**New Chat** を選択して新しい session を開始します。
6. GitHub tools を使えるよう agent picker で **Agent** を選択します。
7. 次のプロンプトを送信します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

8. Copilot が情報取得や GitHub 操作の実行を求めたら **Continue** を選択して許可します。GitHub tool の初回利用時は認証を求められることがあります。画面の案内に従ってください。
9. Copilot が issue を検索して正しいものを見つけ、PR を作成したことを確認します。
10. Copilot が表示したリンクから PR を開いてレビューしますが、まだマージしないでください。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP server が Copilot にツールを追加する仕組みを学びました。
- workspace に Playwright MCP server を追加しました。
- ブラウザーで Playwright MCP server を使い、リリース前にフィルタリング機能を手動テストしました。
- branch を公開し、フィルタリング作業の PR を作成しました。

次は[アクセシビリティ向けのカスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP server][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
