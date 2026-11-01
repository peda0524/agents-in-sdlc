---
title: "レッスン 5 - quality-checks skill をカスタマイズして使う"
description: "既存の quality-checks skill を確認し、報告形式をカスタマイズしてフィルタリング機能を検証します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、必要な作業があります。これまで、コードが動作することを手動で検証し、instruction file を使って標準に従うようにしました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには **agent skill** が適しています。skill は、こうした操作を正しく実行する方法を Copilot に伝えます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` skill を確認します。
- 結果の報告形式をカスタマイズします。
- skill を再読み込みして実行します。

## シナリオ

Tailspin Toys はユニット テスト、lint、型チェックに `quality-checks` skill を使います。チームでは結果を読みやすくするため、報告形式の改善を検討しています。

## 指示、スクリプト、リソース

Agent skill は再利用可能なタスク指示、実行可能なスクリプト、補助リソースをまとめ、必要なときに agent が読み込めるようにします。基本的には、skill 名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown には skill の名前と説明を定義する frontmatter、概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトやほかのリソース用のサブフォルダーも追加できます。

> [!NOTE]
> skill に追加ファイルやフォルダーは必須ではありません。Tailspin Toys の `quality-checks` skill は、プロジェクトに既存のコマンドを使うため、`SKILL.md` のみで構成されています。

skill は、チーム内で共有・再利用するためにプロジェクトの `.github/skills` に配置するか、ユーザーの `~/.copilot/skills` に配置できます。

## Skill を確認する

1. Codespace に戻り、`.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。description は Copilot が skill を呼び出すタイミングを判断するのに役立ちます。
3. テストと lint の手順に関する指示を確認します。
4. **Results output formatting** セクションがまだないことを確認します。

## 変更前に skill を実行する

Copilot CLI から直接 skill を呼び出すことも、自然な言葉で依頼することもできます。3 種類のチェックを実行するよう Copilot に依頼します。

1. フィルタリングの会話に戻り、Interactive モードであることを確認します。
2. 次の prompt を送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

どのコマンドを実行したか、成功したか、ツールがどのような結果を報告したかが分かるレポートにします。

1. `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの最後に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルを保存します。

## 更新した skill を実行する

Copilot CLI は会話を再起動せずに、編集された skill を再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 最初と同じ prompt を使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最初のレポートと比較して、更新された報告を確認します。

## まとめと次のステップ

既存の agent skill をカスタマイズして使いました。ユニット テスト、lint、型チェック用の `quality-checks` skill を調べ、結果の書式を変更し、再読み込みして実行しました。

その変更は機能の PR に含めます。次は [Playwright MCP server][next-lesson] を使って、Copilot がサイトを直接操作できるようにします。

## Skill の例

次のコミュニティ例は参考資料であり、追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と export script: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
