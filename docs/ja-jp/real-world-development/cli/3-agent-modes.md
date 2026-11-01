---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue からフィルタリングを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しました。より大きな変更には、堅実なプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作します。このレッスンでは issue から機能を実装し、コードの妥当性と機能の動作を確認して、プロジェクトにマージするまで進めます。

> [!NOTE]
> 機能の作業中は、同じ会話と branch を使います。通常はファイル種別ごとに異なる branch や PR を使いますが、ここでは主な概念に集中するため手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を始めます。
- Plan モードで要件を決めます。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送ブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリ指示を更新し、`quality-checks` skill をカスタマイズし、MCP による検証を追加し、QA エージェントを作成し、feature PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、利用者がカテゴリとパブリッシャーでゲームを絞り込める必要があります。バックログの issue には機能が記載されていますが、カテゴリを組み合わせる方法などはコードを書く前に合意が必要です。Plan モードで決定し、Autopilot に範囲を限定した実装を許可します。

## 背景

AI コーディング エージェントを開発に取り入れても、基本的な工程は変わりません。

1. 作業内容を記載した issue から始めます。
2. 実装計画を立てます。
3. コードを作成してレビューします。
4. テストを実行して検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. レビューと継続的インテグレーションが成功したらマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れを調整したものになります。

この標準的な手順に沿うことで、AI が生成したコードが要件を満たし、手書きのコードと同じレビューと検証を受けるようにできます。

## 会話モード

**Conversation mode** は、エージェントに与える自律性の度合いを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、続行前に入力を待ちます。
- **Plan**: エージェントが計画を作成し、プロジェクト ファイルの編集は行いません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画をレビューし、その後 Autopilot で実装します。

## issue から始める

1. Codespace に戻ります。停止中の場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用 branch を作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトには `#7` などの issue 番号が表示されます。`#` を使うと GitHub 上の issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルタリング機能を計画する

複雑な機能を実装する前に、計画を立てて方針を決めます。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替え、プロンプトの下に **Plan** と表示されていることを確認します。
2. 直前の手順で追加した issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、実装方法を提案します。

3. フィルタリングの動作について Copilot から質問されたら、希望に沿って回答します。正解は一つではありません。
4. 計画が完成したら <kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画をレビューしたら実装に進みます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みバージョンで同等の項目を選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 承認すると実装がすぐに始まる場合があります。先に計画をレビューしてください。依存関係の不足やポートの競合が報告された場合は、チェック完了とみなす前にセットアップの問題を解決します。

## 実装をレビューして検証する

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに戻り、モード表示に **Plan** や **Autopilot** が出ていないことを確認します。
2. `/diff` を入力し、フィルタリングの実装とテストを確認します。
3. issue と計画時の決定に照らし合わせます。
4. レビュー後、<kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

1. 次のプロンプトで Copilot にアプリを起動させます。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. カテゴリ、パブリッシャー、および計画時に合意した組み合わせで絞り込みます。
4. リセットと結果がない場合の動作を issue と決定に照らして確認します。
5. Codespace に戻り、Copilot に起動したサーバーの停止を依頼します。

## まとめと次のステップ

会話モードを使い分けて機能を作成し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub issue から新しい Copilot の会話を始めました。
- Plan モードで要件を決めました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送ブラウザーで機能を手動検証しました。

次は[カスタム指示を使ってコード生成をさらに詳しく確認します][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
