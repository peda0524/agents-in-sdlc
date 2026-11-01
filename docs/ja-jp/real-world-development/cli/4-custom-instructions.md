---
title: "レッスン 4 - カスタム指示による Copilot のガイド"
description: "リポジトリの指示を確認し、ドキュメント標準を追加してフィルタリング コードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合、そのガイダンスを Copilot が参照できるようにします。[Instruction file][instruction-files]では、必要なコードだけでなく、その構成方法も説明できます。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス単位の instruction file がエージェントにどう適用されるかを確認します。
- コーディング標準が守られるよう instruction file を更新します。
- instruction file がコードに与える影響を確認します。

## シナリオ

Tailspin Toys には開発ガイドラインがあります。

- コメントはコードを言い換えるのではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` の公開関数には、目的、パラメーター、戻り値を TSDoc/JSDoc で記載します。`db` 引数を注入する場合は、その引数も文書化します。
- 再利用可能な Astro component は `Props` の契約を文書化し、関連コードが変わったらコメントも更新します。
- 既存の書式と lint のガイダンスを維持します。

instruction file を使い、Copilot がこれらのプラクティスに沿うようにします。

## Instruction file

Custom instructions は、コーディング規約、ライブラリ、コメントの書き方などのコンテキストを Copilot に伝えます。リポジトリ全体に適用する指示と、特定の言語、ファイル種別、タスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべての依頼に送られる指示です。技術スタック、プロジェクトの概要、ベスト プラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、言語やファイル種別、特定のタスク向けのガイドラインを記載します。

> [!NOTE]
> 環境によって、instruction file の形式とサポート状況は異なります。特定の形式に依存する前に、[カスタム指示のサポート リファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、変更前に調べられる instruction file が含まれています。

1. Codespace に戻ります。
2. Codespaces editor で `.github/copilot-instructions.md` を開きます。プロジェクトの簡潔な説明と、リポジトリ内の Copilot とのすべてのやり取りに適用されるコーディング ガイダンスを確認します。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob に一致するファイルに指示が適用されます。
5. `.github/instructions/drizzle.instructions.md` を開き、ほかの instruction file や既存のプロジェクト ファイルへのリンクを確認します。これにより、指示を小さく再利用しやすいファイルに分け、Copilot が参照できる例を示せます。

## instruction file を更新する

このプロジェクトの instruction file を使い始めるため、まず既存のガイダンスを確認し、次に TSDoc の標準を追加してフィルタリング コードへ適用します。

1. Codespace に戻り、`.github/copilot-instructions.md` を開きます。
2. **Code formatting guidance** セクションを見つけます。現在はコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。
3. そのセクションの最後に、次の項目を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. ファイルを保存します。

プロジェクト全体の情報を `.github/copilot-instructions.md` に、特定の言語、ファイル種別、タスクのガイドラインを `.github/instructions/*.instructions.md` に記載します。自然な言葉で簡潔に書き、良い例と悪い例を示してください。プロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] の指示があります。テンプレートを探す場合は [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの instruction file を読み込みます。更新した指示を Copilot に適用しつつ、フィルタリング機能のコンテキストを保つため、保存済みの会話を再開します。

1. フィルタリングの会話が開いている場合は `/exit` で終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` branch の repository root から、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

3. 選択を求められた場合は、フィルタリングの会話を選び、`game-filters-cli` branch がチェックアウトされていることを確認します。
4. Copilot に、更新後のガイダンスを適用するよう次の prompt で依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

5. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が instruction file からコンテキストを取得する方法を確認し、リポジトリ標準を追加してフィルタリング機能に適用しました。次は[再利用可能な `quality-checks` skill をカスタマイズして使います][next-lesson]。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示作成のベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
