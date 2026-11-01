---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方や Copilot に知っておいてほしい背景情報がある場合は、必要なガイダンスを参照できるようにします。[Instruction file][instruction-files]を使うと、何をするかだけでなく、どのように進めるかも Copilot に伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の instruction file を使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot にどのように伝わるかを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用される標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub issue を非同期に処理するため、リアルタイムで並べて効果を示しにくいためです。後の演習で cloud agent が作成するプルリクエストをレビューするときに、指示の影響を確認します。

## Instruction file

### シナリオ

Tailspin Toys には開発プラクティスのガイドラインがあります。

- データ レイヤーには必ずユニット テストを用意します。
- UI はモダンな印象のダーク モードにします。
- TSDoc doc comment をコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

instruction file を使い、Copilot がこれらのプラクティスに従えるようにします。

### Custom instructions

Custom instructions は、コーディング規約、ライブラリ、コメントの書き方などのコンテキストや設定を Copilot に伝えます。リポジトリ全体に適用する指示と、特定のタスクやファイル種別に適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべての依頼に適用される単一の instruction file です。技術スタック、プロジェクトの概要、ベスト プラクティスなど、全般的な情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われますが、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく instruction file もサポートしています。

## Instruction file を管理するためのベスト プラクティス

このワークショップでは instruction file 作成の全ては扱いませんが、サンプル プロジェクトには代表的な構成例があります。

- `copilot-instructions.md` には、構築するものの説明、プロジェクト構造、全体のコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` は、ユニット テスト、Astro component、データ レイヤーなどのファイル種別や、特定のタスク向けに使います。
- 自然な言葉で明確に記述します。望ましいコードと望ましくないコードの例を示します。

instruction file の作成方法も AI の使い方も一つに決まっているわけではありません。プロジェクトに合う方法を試してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、作業に応じた instruction file を用意できます。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions] の指示があります。
>
> Copilot に instruction file を生成させる方法もあります。VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` など、利用画面ごとに操作が異なります。テンプレートを探す場合は、instruction file、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を参照してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## このプロジェクトの指示を確認する

1. `.github/copilot-instructions.md` を開き、プロジェクトの説明、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。
2. `.github/instructions` フォルダーを開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
3. `.github/instructions/unit-tests.instructions.md` の `applyTo` フィールドを確認します。repo root からの相対 glob に一致するファイルに指示が適用されます。
4. `.github/instructions/drizzle.instructions.md` を最後まで読み、ほかの instruction file やプロジェクト内ファイルへのリンクを確認します。そこに記載されたパスは instruction file からの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次に TSDoc doc comment とファイル コメント ヘッダーの規則を追加します。

## リポジトリ標準を追加する

cloud agent は、issue が対象とするブランチの instruction file を読み込みます。次の演習では issue を `main` に割り当てるため、変更を `main` にマージしておきます。

1. codespace のターミナルでブランチを作成します。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. `.github/copilot-instructions.md` を開き、**Code formatting requirements** セクションの下に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. ファイルを保存し、変更を commit して push します。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   git push -u origin custom-instructions-cloud
   ```

4. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成し、マージします。

> [!TIP]
> このワークショップではブランチを使いますが、直接 `main` にコミットして進めることもできます。ブランチを使う手順は、実際のプロジェクトでの流れを再現しています。

## まとめと次のステップ

instruction file が Copilot にコンテキストを与える方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りのハーネスでは、cloud agent が作成するプルリクエストでこの標準を確認します。

次は[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をより良く書く 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — instruction file などのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
