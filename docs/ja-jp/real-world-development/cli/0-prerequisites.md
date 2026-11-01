---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、Tailspin Toys リポジトリの自分用コピーを作成し、次のレッスンで Copilot CLI をインストールして実行するための統合ターミナルを備えた [Codespace][codespaces] を起動します。

このレッスンでは、次のことを行います。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。
- Codespace を作成し、プロジェクトの準備ができていることを確認します。

## ラボ用リポジトリをセットアップする

Tailspin Toys プロジェクトの自分用コピーを使います。[テンプレート リポジトリ][tailspin-template]から作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template**、次に **Create a new repository** を選択して、自分用のコピーを作成します。
3. GitHub または Microsoft 主催のイベントで参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。
4. 後から参照できるよう、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動的に作成されます。ワークショップではこの issue を使うため、自分で登録する必要はありません。

このワークショップのテンプレートには、リポジトリの指示、アプリケーションのコード、テスト、`quality-checks` skill、バックログが含まれています。新しいコピーを使ってください。古いコピーを使用する場合は、必要なファイルが含まれていることをファシリテーターに確認します。

## Codespace を作成する

1. 作成したリポジトリを開き、**Code** を選択します。
2. **Codespaces** タブを選び、**Create codespace on main** を選択します。
3. Codespace のセットアップが完了するまで待ちます。テンプレートが依存関係、Playwright Chromium、ローカル データベースをインストールします。
4. **Do you trust the authors of the files in this folder?** と表示された場合は **Trust Folder & Continue** を選択します。
5. repository root のターミナルを開き、アプリを起動します。

   ```bash
   npm run dev
   ```

6. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys が読み込まれることを確認します。
7. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

Tailspin Toys の自分用コピーを作成し、Codespace を準備して、プロジェクトが動作することを確認しました。次は Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
