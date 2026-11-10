---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本工作坊中，你与 GitHub Copilot 一起完成了多项改善用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈，进一步改进结果。

### 场景

如前所述，生成式 AI 不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议，再启用团队审查。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体异步且无人监督地执行任务，因此需要采取一些安全限制：

- Copilot 只能读取存储库，并且只能写入它用于修改代码的分支。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准后才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。通过 github.com 上的标准 PR 界面审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问包括外部 MCP 服务器在内的外部资源。可以[自定义或关闭云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时等待 Copilot 完成即可，可能需要几分钟。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新生成的 TSDoc 文档注释及其他文档。实际更改会因 Copilot 生成结果而异。

   检查 TSDoc 注释、TypeScript 约定和注释头。这些来自练习 2 开始时查看的自定义说明文件。

6. 审查完毕且确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域中。顺利的话，单体 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像在团队 PR 中一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新会话处理反馈。由于生成结果具有概率性，不宜规定固定的反馈文本，但可以要求 Copilot：

- 在每个代码文件顶部添加简要说明用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 在评论中使用 **@copilot** 提出文档更改请求。可以使用以上建议，或提出其他希望加入代码库的文档改进。
2. 选择 **View Session**，查看 Copilot 如何工作。它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。之后可以将 PR 从草稿状态改为准备审查，并合并到主分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完毕且确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会出现在 PR 的检查区域中。顺利的话，单体 Astro 应用的项目检查将通过；此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，可以继续迭代并请求云智能体更新，例如调整显示的相关游戏数量，或补充注释头和 TSDoc 文档注释。请记住，分配该任务时还没有更新自定义说明。可以像之前一样在 **Conversation** 选项卡发表评论，让 Copilot 继续处理。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 查看实现，尤其要检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储持久化用户偏好。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完毕且确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 的检查区域中。顺利的话，单体 Astro 应用的项目检查将通过；此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

注意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践。如果发现问题或可以改进的地方，可以像之前一样在评论中提及 **@copilot** 并请求更新。

## 可选练习——继续委派工作

云智能体很适合处理待办事项，让你能同时开展其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，养成这种工作习惯。示例：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成云智能体工作流。本系列课程中，你：

- **检查了本存储库自带的自定义说明文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云智能体创建的拉取请求**，使用 **@copilot** 请求更改并批准工作流。

## 回顾和后续步骤

云智能体工作流已完成。其他工作流可以帮助你继续探索：

- 🖥️ **[VS Code 工作流](../../vscode/)**——在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流](../../cli/)**——在终端交付两项经过审查的更改，使用计划和自动驾驶模式、智能体技能、QA 自定义智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
