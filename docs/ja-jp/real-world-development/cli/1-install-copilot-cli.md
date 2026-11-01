---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルタリング issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディング アシスタントです。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールを利用できます。タスクを任せたり、変更を依頼したりしながら、コマンド ラインで作業を続けられます。まずはツールをインストールしましょう。

このレッスンでは、次のことを行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP server でフィルタリング issue を見つけます。

## シナリオ

チームでは増え続けるバックログに対応するため、AI agent を使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでインストールと認証を済ませ、残りのワークショップで使えるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js がプリインストールされているため、npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 認証を求められた場合は、device code の手順に従って認証し、Copilot CLI を許可します。
3. Copilot CLI に次の確認が表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では GitHub セッションを通じて認証済みの場合があります。認証を求められずに Copilot CLI が起動すれば、そのまま進められます。

## 基本を確認する

通常の shell prompt で入力したコマンドは Codespace で実行されます。Copilot CLI の起動後は、自然言語を agent に送り、slash command で会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> で確定します。
2. `/help` でインストール済みバージョンのコマンドを確認し、<kbd>Esc</kbd> でヘルプを閉じます。
3. 次の質問で動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. Copilot が回答前にリポジトリを調べる様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP server が利用できることを確認します。
6. 次のプロンプトでフィルタリング issue を探します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、issue を読みます。最初の変更が終わったらこの issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしている branch を使います。自動的に worktree は作成されません。変更を始める前に feature branch を作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可された範囲外のツールを使う前に確認を求めます。このワークショップでは Codespace 内で `--yolo` を使い、承認の確認を省略して演習に集中します。

> [!CAUTION]
> `--yolo` は完全な自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使い、日常の開発で既定値として alias にしないでください。詳細は [Allowing and denying tool use][allow-all-warning] を参照してください。

`--enable-all-github-mcp-tools` は、後続レッスンで issue やプルリクエストを操作するために必要な GitHub MCP の読み書きツールを有効にします。Codespace はローカル コンピューターへのアクセスを制限しますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. repository root から再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 簡単な質問をして会話を確認し、`/exit` で終了します。

Copilot は会話を自動保存します。後で instruction を変更したり agent を追加したりした後、`copilot --resume` で同じ会話と branch に戻ります。

## まとめと次のステップ

npm で Copilot CLI をインストールし、GitHub アカウントで認証し、ワークショップのリポジトリを信頼して簡単な会話を試しました。組み込みの GitHub MCP server でフィルタリング issue も見つけました。

次は[最初の対象を絞った変更][next-lesson]として、Copilot CLI でゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
