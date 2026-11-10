---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你要求 Copilot 云智能体分别处理三个改善用户体验和添加功能的任务。云智能体虽然异步且自主运行，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，可以查看所有创建了未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，并引导正在进行的会话。

本课将：

- 浏览智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加额外功能。

## 场景

将实现高对比度模式的任务分配给智能体后，团队意识到也应该增加浅色模式。由于智能体已经在更新网站样式并添加切换功能，顺便实现浅色模式也很合理。现在引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的所有任务的当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。其中应包括已分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 创建 Copilot 工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时查看右侧显示的 PR 面板，以便监控 PR 和更改的文件。

## 引导云智能体

查看活动任务后，要求 Copilot 在实现高对比度模式时一并增加浅色模式切换开关。

1. 选择负责添加高对比度模式的会话。会话标题会因 Copilot 采用的名称和当前工作状态而异。

   ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到 Copilot 完成设置并开始工作。之后会看到类似的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它可能会先完成当前步骤，再将新请求加入流程。

这可能需要几分钟。等待时，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。在这里可以跟踪所有活动和已完成的任务、审查工作内容，并重定向正在进行的任务以调整范围或补充指导。

本课介绍了：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求增加功能。

Copilot 正在完成功能实现。接下来[迭代改进 Copilot 创建的拉取请求][next-lesson]。

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
