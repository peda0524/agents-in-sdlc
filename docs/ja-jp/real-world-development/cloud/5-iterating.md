---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、コードへのドキュメント追加、関連ゲーム機能の実装、ハイ コントラストとライト モードの追加を Copilot に依頼しました。変更内容を確認し、必要に応じてフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計や DevOps の基本は変わりません。コードをレビューし、通常の DevOps プロセスに沿って進めます。チームにレビューを依頼する前に、ドキュメント、関連ゲーム、アクセシビリティ機能の提案を確認します。

## GitHub Copilot cloud agent のセキュリティ

cloud agent は非同期かつ自律的に作業するため、安全性を保つ仕組みがあります。

- Copilot はリポジトリに読み取りアクセスでき、書き込みは作業用ブランチに限られます。
- cloud agent は GitHub Actions の一時的な環境で実行されます。
- GitHub Actions ワークフローは、人が承認するまで実行されません。
- [外部リソースへのアクセス][agent-firewall]は、MCP サーバーを含め既定で制限されています。

## 生成されたドキュメントをレビューする

1. github.com でリポジトリを開き、**Pull Requests** を選択します。
2. `Add missing documentation` のようなタイトルの PR を開きます。Copilot がまだ作業中で `[WIP]` と表示されている場合は、完了まで待ちます。
3. **Files changed** タブを選び、変更を確認します。TSDoc doc comment、TypeScript 規約、ファイル コメント ヘッダーを確認してください。
4. **Conversation** タブに戻り、ワークフローが承認待ちの場合は **Approve and run workflows** を選びます。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

5. PR の checks セクションでワークフローが開始されたことを確認します。単一の Astro app のプロジェクト チェックが成功するまで待ちます。数分かかる場合があります。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。外部リソースへのアクセスは既定で制限されているため、これは想定された動作です。必要であれば、cloud agent のファイアウォールを[設定][agent-firewall]できます。

### GitHub Copilot に変更を依頼する

PR のコメントやコードの行コメントで `@copilot` に変更を依頼できます。Copilot はコメントを確認し、変更に対応する新しいセッションを開始します。依頼する内容を厳密に決める必要はありません。たとえば、各ファイルに役割を説明するコメントを追加する、TypeScript と Astro に TSDoc doc comment を追加する、Astro app の README を作成する、といった依頼ができます。

1. PR に `@copilot` を含むコメントを追加して変更を依頼します。
2. **View Session** を選択し、Copilot が新しいセッションで変更する様子を確認します。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

3. PR に戻り、新しい commit と **Files changed** の差分をレビューします。納得できるまで反復し、完了後にドラフトをレビュー可能にしてマージします。

![ドラフト PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

1. リポジトリの **Pull Requests** から `Show related games on the game details page` のようなタイトルの PR を開きます。
2. **Files changed** を確認し、問題がなければ **Conversation** に戻ります。
3. ワークフローの承認が必要な場合は **Approve and run workflows** を選びます。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

4. PR の checks セクションでワークフローが開始され、プロジェクトのチェックが成功するまで待ちます。数分かかる場合があります。
5. 必要に応じて Codespace で Copilot のブランチに切り替え、手動でテストします。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

作業を追加で依頼するには、PR の **Conversation** タブにコメントします。たとえば関連ゲームの表示件数を変えたり、instruction を更新する前に割り当てたタスクへコメント ヘッダーや TSDoc を追加したりできます。

## アクセシビリティ機能をレビューする

この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 で追加依頼したライト モードが含まれます。

1. `Add high contrast mode to website` のようなタイトルの PR を開きます。`[WIP]` が付いている場合は、完了まで待ちます。
2. **Files changed** で切り替え UI、ユーザー設定の local storage への保存、配色、ARIA 属性、キーボード操作、モード切り替えコードを確認します。
3. **Conversation** タブに戻り、ワークフローが承認待ちの場合は **Approve and run workflows** を選びます。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

4. PR の checks セクションでワークフローが開始され、プロジェクトのチェックが成功するまで待ちます。数分かかる場合があります。
5. 必要であれば Codespace でブランチをテストします。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ハイ コントラスト モードとライト モードを切り替えます。ページを再読み込みしても設定が保持されることを確認してください。

アクセシビリティ エージェントは、アクセシビリティのベスト プラクティスに沿って作業を案内します。懸念や改善点があれば、ほかの PR と同様に `@copilot` を含むコメントで依頼します。

## 省略可能な演習 — さらに作業を委任する

cloud agent に実際のバックログを割り当て、別の作業に取り組む方法を試します。たとえば、ゲーム詳細ページの支援者向けフォーム、ゲーム一覧のページネーション、データアクセス helper の入力検証やエラー処理を依頼できます。

## まとめ

Cloud agent ハーネスを完了しました。

- リポジトリのカスタム instruction file を確認し、cloud agent の出力への影響を調べました。
- issue を Copilot cloud agent に割り当て、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ向けのカスタムエージェントを使い、ハイ コントラストとライト モードを追加しました。
- Copilot agents ページでセッションを監視し、作業中に指示を出しました。
- cloud agent が作成した PR をレビューし、`@copilot` で変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスは完了です。ここで学んだことを補完する内容として、[VS Code ハーネス](../../vscode/)では Copilot Chat の agent mode と MCP 統合を IDE で確認でき、[CLI ハーネス](../../cli/)では Plan と Autopilot、agent skill、QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、リファクタリングやテスト カバレッジの issue を割り当てたり、別分野のカスタムエージェントを作成したり、別の技術スタック向けに `copilot-setup-steps.yml` を設定したりしてみましょう。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent セットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
