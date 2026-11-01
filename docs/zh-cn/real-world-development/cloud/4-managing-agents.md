---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两节练习中，你让 Copilot cloud agent 处理了三项旨在改善用户体验和添加功能的任务。Cloud agent 虽然会异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制面板中，可以查看所有创建了开放拉取请求 (PR) 的智能体任务，了解已执行的操作，甚至在会话进行期间引导智能体。

本课将完成以下事项：

- 探索智能体页面，监控 cloud agent 任务。
- 在会话进行期间引导智能体，要求添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队发现还可以同时添加浅色模式。既然智能体正在更新网站样式并添加切换功能，顺便加入浅色模式也很合理。现在引导智能体，确保它同时添加高对比度和浅色模式。

## 查看 Copilot cloud agent 任务

现在查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表，其中应包含分配给 Copilot 的任务，例如：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 具体显示的步骤因 Copilot 工作状态和采用的方案而异。

4. 同时留意右侧的拉取请求 (PR) 面板。你可以在这里查看 PR 和已更改文件，进一步监控任务。

## 引导 cloud agent

查看当前正在运行的任务后，现在请 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择负责添加高对比度模式的会话。会话标题因 Copilot 使用的名称和当前工作状态而异。

    ![智能体控制面板中的 Accessibility 会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。看到类似下方的消息时，就表示已经开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的 cloud agent 任务，突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！它会先完成收到消息时正在执行的步骤，再将新请求加入工作流程。

这也需要几分钟，可以趁此暂停片刻，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心位置。你可以在这个控制面板中跟踪所有正在进行和已完成的任务、审查工作内容，并重新引导进行中的任务以调整范围或补充说明。

本课中，你了解了：

- 如何在控制面板和智能体页面监控 cloud agent 任务。
- 如何重新引导进行中的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，开始[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
