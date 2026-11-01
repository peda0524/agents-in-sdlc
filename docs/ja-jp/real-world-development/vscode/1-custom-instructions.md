---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot に伝えたい背景情報がある場合は、必要な情報を参照できるようにします。[Instruction file][instruction-files]を使うと、コード生成の指針を伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス単位の instruction file が、プロジェクト固有のコンテキストや標準を Copilot に伝える方法を確認します。
- 現在の指示を使って、フィルタリングに必要な publishers helper を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を取り入れる様子を確認します。
- 指示の更新と helper を `main` に commit し、次の演習から使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、指示の更新後に動作がどう変化するかを確認します。出力を 1 文字ずつ一致させることが目的ではありません。

## Instruction file

### シナリオ

Tailspin Toys には開発ガイドラインがあります。データ レイヤーには必ずユニット テストを用意すること、UI をモダンなダーク モードにすること、TSDoc doc comment をコードに追加すること、各ファイルの先頭に役割を説明するコメントを追加することなどです。instruction file を使い、Copilot がこれらのプラクティスに沿うようにします。

### Custom instructions

Custom instructions は、コーディング規約、ライブラリ、コメントの書き方などのコンテキストを Copilot に伝えます。リポジトリ全体に適用する指示と、特定の言語、ファイル種別、タスクに適用する指示を作成できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべての依頼に適用されます。技術スタック、プロジェクトの概要、ベスト プラクティスなどを記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けの指示です。

> [!NOTE]
> IDE では、instruction file は Copilot Chat のコード生成に使われますが、コード補完や next-edit suggestion には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく instruction file もサポートしています。

`copilot-instructions.md` にはプロジェクト全体の指針を、`*.instructions.md` にはユニット テストや Astro component、データ レイヤーなどのファイル種別やタスク固有のルールを記載します。自然な言葉で簡潔に書き、良い例と悪い例を示してください。テンプレートを探す場合は [awesome-copilot][awesome-copilot] を参照してください。

instruction file はプロジェクト固有のコンテキストを分かりやすく分割するのにも役立ちます。たとえば、このプロジェクトの指示には Astro、Tailwind CSS、Drizzle 向けのファイルがあり、ほかの instruction file やプロジェクト内の例を参照します。instruction file の作成に唯一の正解はありません。プロジェクトに合う方法を試してください。

> [!TIP]
> プロジェクトの指示には、[UI 更新][ui-instructions]や [Astro][astro-instructions] などのタスクやファイル種別に応じた例を含められます。Copilot に instruction file を作らせる方法は画面ごとに異なり、VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` などがあります。テンプレートは [awesome-copilot][awesome-copilot] で探せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization

## このプロジェクトのカスタム指示を確認する

1. `.github/copilot-instructions.md` を開き、プロジェクトの概要、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** を確認します。**Code standards** 内の **GitHub Actions Workflows** の指示にも注目します。
2. `.github/instructions` を開き、Astro、Drizzle データ レイヤー、テストなどの指示を確認します。
3. `.github/instructions/unit-tests.instructions.md` の `applyTo` フィールドを確認します。repo root からの相対 glob に一致するファイルに指示が適用されます。
4. `.github/instructions/unit-tests.instructions.md` を読み、プロジェクトのユニット テストに固有の指示も確認します。
5. `.github/instructions/drizzle.instructions.md` を最後まで読み、`unit-tests.instructions.md` などの instruction file や既存のプロジェクト ファイルへのリンクを確認します。パスは instruction file からの相対パスです。指示を再利用しやすい小さなファイルに分け、Copilot に従う例を示せます。

> [!NOTE]
> `.github/copilot-instructions.md` の **Code formatting requirements** にはコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次に TSDoc doc comment とファイル コメント ヘッダーの標準を追加します。

## 変更用 branch を作成する

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. branch を作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## instruction を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の手順を始める前に codespace に戻り、Copilot Chat を開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

1. 前の演習のタブを閉じ、Copilot に必要なコンテキストだけを与えます。
2. `src/lib/publishers.ts` を開き、helper の配置場所を Copilot に知らせます。
3. Chat ビューの agent picker で **Agent** を選びます。
4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

Copilot はプロジェクトを調査してコードを更新します。helper が型付き関数で、最初の引数として `db` client を受け取り、型付き publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ レイヤー規約に基づいています。生成されたコードには TSDoc doc comment とファイル コメント ヘッダーがまだ含まれていないことを確認します。

Copilot は helper とそのテストなど、複数のファイルを更新することがあります。Copilot が確率的に動作するため、指示を追加する前からコメントが含まれる場合もあります。その場合も問題ありません。ここでは、標準を追加した後に挙動がより一貫するかを確認します。

> [!CAUTION]
> Copilot が指示なしに doc comment を追加する場合もあります。その場合も問題ありません。重要なのは指示を更新した後の一貫性です。

## リポジトリ標準を追加する

1. `.github/copilot-instructions.md` を開きます。
2. **Code formatting requirements** セクションの下に次の行を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

3. `copilot-instructions.md` を保存します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat で **New Chat** を選びます。
2. `src/lib/publishers.ts` を開き、Copilot の対象ファイルにします。
3. agent picker で **Agent** が選択されていることを確認します。
4. 前と同じプロンプトを再送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. ファイルの先頭に、次のようなコメントがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

6. 関数に次のような TSDoc があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで新しい標準を Copilot に適用し、次の演習で使うコードに反映できました。

## 指示と branch を commit して push する

instruction file もほかの repository ファイルと同様に source control で管理します。変更と helper の foundation を commit します。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) で新しいターミナルを開きます。
2. 変更を確認します。

   ```bash
   git status
   ```

3. instruction の更新、helper、および Copilot が追加した関連ファイルを stage して commit します。テストなどの追加ファイルが `git status` に表示される場合は、それらも追加します。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. branch を repository に push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

push した branch から PR を作成し、指示の更新と今後のフィルタリング作業に使う publishers helper foundation の両方を説明します。GitHub の issue を探して PR に関連付けるよう Copilot に依頼します。

1. Copilot Chat を開きます。<kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押し、agent picker で **Agent** を選択します。
2. 次の prompt を送信します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

3. Copilot が issue を探し、PR を作成する様子を確認します。GitHub 操作の許可を求められたら、コマンドを確認してから **Allow** を選択します。
4. PR が作成されたら、次の prompt でマージしてローカルの `main` を更新するよう依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

5. GitHub 操作や shell command の許可を求められたら、内容を確認してから **Allow** を選択します。

これで Copilot Chat を使って PR を作成し、マージできました。

## まとめと次のステップ

Copilot Chat で既存の指示を使ってコードを生成し、`.github/copilot-instructions.md` に新しい標準を追加した後、同じプロンプトを再実行しました。次は[エージェント モードで指示を機能に適用します][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用 instruction file][instruction-files]
- [カスタム指示作成のベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をより良く書く 5 つのヒント][copilot-instructions-five-tips]
- [個人用のカスタム指示][personal-instructions]
- [Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
