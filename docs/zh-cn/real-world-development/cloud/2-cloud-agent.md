---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。智能体完成后会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保操作安全执行。
- 了解范围明确的议题为何重要。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下合适的文档，事实上，代码完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页上显示同类别的其他游戏，让用户可以继续浏览。目前不要求实现经过润色的最终方案，只需提供可用于验收测试的用户体验。这是当前的阻塞项，不过还有其他优先级更高的议题。

这两项工作都可能很快被推迟，因此适合分配给 Copilot cloud agent。它可以异步处理工作，让开发者先专注于其他任务，稍后再审查结果并确认一切符合预期。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像开发者一样在后台执行任务。你可以通过多种方式启动工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置工作环境并开始处理任务。你可以在 Copilot 会话运行期间或完成后查看详情。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法工具，不能替代思考。即使看似简单的操作，也可能有许多隐藏的复杂性，因此请谨慎设定 Copilot cloud agent 的任务范围，并参考[任务范围最佳实践][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段工作、不断学习和试验，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

之前的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成过自定义说明的动手练习，就会看到 `.github/copilot-instructions.md` 的效果；如果还没看过，现在正是快速浏览的好时机。将任务分配给 Copilot cloud agent 前，先以只读方式查看存储库中已有的说明文件，之后就能识别它们的影响。

在 GitHub 网站上打开存储库中的以下文件；如果已启动 Codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给 cloud agent。留意随后生成的拉取请求中的 TSDoc 文档注释、TypeScript 约定和文件注释标题；这些内容来自说明文件。之后的练习审查 PR 时还会再次讨论。

## 为 Copilot cloud agent 设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保开发环境状态良好。将任务分配给 Copilot 时也是如此，因为它会以类似软件工程师的方式工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境执行任务。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，并在 Copilot 开始工作前运行。这样可以提供所需的开发工具和依赖项。实验室已预先配置此工作流，以便顺利完成练习。它会确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用中已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为执行这些步骤所需的最低级别。Copilot 会为自身操作使用单独的令牌。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如为了安装依赖项），需要 `contents: read` 权限。如果不在设置步骤中克隆，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，供构建和测试使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要留意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但许多项目中的信息已经过时，或者完全缺少文档。这类技术债务经常无人处理，会拖慢开发效率，让代码库更难维护，也让新开发者更难上手。Copilot 擅长创建文档，因此很适合将这项任务分配给 Copilot cloud agent。它会在后台生成所需文档，之后的练习中你会回来审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 标题输入 `Code lacks documentation`。
6. 描述输入：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建任务列表。

> [!NOTE]
> Copilot 创建的任务列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳解决方案。

13. 查看任务列表及 Copilot 准备执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并选择合适的方案，以及编写代码。

此过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们可以先处理其他工作。现在就利用这一点：再将一个任务分配给 Copilot cloud agent，然后继续为应用编写其他功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，同时让它处理其他任务。即使为设计团队添加相关游戏部分并不需要太久，这段时间也可以用于其他工作。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 标题输入 `Show related games on the game details page`。
6. 描述输入：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论旁会出现一组 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在努力处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此无需一直监控；等它完成后再审查即可。现在来创建并使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。你可以将议题分配给 cloud agent，让它异步处理任务。Copilot 可以用于处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课中，你了解了：

- 如何自定义代码生成环境。
- 如何确保操作安全执行。
- 范围明确的议题为何重要。
- 如何将议题分配给 Copilot。

Cloud agent 正在后台工作，现在可以开始创建和使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
