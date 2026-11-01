---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッション共有、cloud agent への任意の委任に使う slash command を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使う slash command が多数あります。高度な機能、内部情報、追加の設定を利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここではほかの便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で会話コンテキストを確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有します。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローを終えたので、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への委任などを試します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて自動的に会話を圧縮します。slash command を使って、状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が開いていない場合は repository root から起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域の内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. 再度 `/context` を入力して比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると Copilot CLI が自動的に圧縮します。タイミングを選びたい場合は `/compact` を使います。別のタスクを始める場合は `/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルごとに得意分野が異なります。Copilot CLI では利用できるモデルを表示し、選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと利用状況を確認します。
3. 現在のモデルを使うか、別のモデルを選ぶか、<kbd>Esc</kbd> で一覧を閉じます。

## セッションを共有する

チームで学びを共有すると、AI ツールの活用を改善できます。`/share` は、セッションを Markdown または HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みバージョンの `/share` オプションを確認します。
2. 共有する場合は、ローカル Markdown 出力の `/share file` など、適切な方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

共有リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業の拡張や委任

ワークショップの主要な内容は完了です。Copilot CLI には、より大きな作業に使えるコマンドもあります。

- `/fleet` は独立したサブタスクを subagent に分割し、並行して実行します。
- `/worktree` は別のタスク用に独立した Git worktree を作成します。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期に作業して PR を作成できます。

これらのコマンドは追加の worktree やリモート作業を作成する場合があるため、任意の操作です。試すときは、範囲が明確な新しいタスクから始め、通常の手順で結果をレビューします。非同期エージェントの詳細は[cloud agent ワークショップ][cloud-workshop]を参照してください。

## まとめと次のステップ

slash command を使うと、Copilot CLI の設定やセッション共有、内部状態の確認ができます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` で Copilot の会話コンテキストを確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有できることを学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

最後に[学んだことを振り返り、次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
