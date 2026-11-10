---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成工作后，它会创建草稿拉取请求，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发进程，让开发人员专注于更重要的工作。

本课将使用 Copilot 云智能体探索：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何为 Copilot 分配议题。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队想改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（同一类别中的其他游戏），方便用户继续浏览。目前不要求精致实现，只需满足用户体验验收测试。但这项工作目前受阻，优先级低于其他议题。

这些任务很容易被搁置，适合分配给 Copilot 云智能体。它可以异步完成任务，让开发人员先处理其他工作，之后再回来审查并确认结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好接受审查时，Copilot 会在拉取请求中通知你。

## 明确界定说明和任务范围

GitHub Copilot 并非魔法，不能替代思考。即使看似简单的操作，深入分析后也会发现复杂性，因此请谨慎[界定分配给云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 编程伙伴：分阶段工作、学习、尝试并不断调整。生成式 AI 不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；否则，可以在此快速查看。分配任务前，以只读方式查看存储库中的说明文件，之后便能辨认它们的影响。

在 GitHub Web UI 或已运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它创建的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自说明文件，后续练习审查 PR 时还会再次介绍。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本，确保环境正常。将任务分配给 Copilot 时也是如此，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]，在任务开始前运行准备步骤，从而提供所需开发工具和依赖项。此练习已预先配置该工作流，确保 Copilot 可以使用 Node.js、项目依赖项、端到端测试所需的浏览器，以及为单体 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果需要在设置步骤中克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它与其他 GitHub 工作流文件类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。在等待 Copilot 运行工作流前，可以先验证它是否成功。

## 添加文档

大家都知道文档很重要，但多数项目的文档要么过时，要么完全缺失。这类技术债务常常无人处理，会降低效率，增加代码维护和新成员入职的难度。Copilot 擅长生成文档，因此很适合将这项工作分配给云智能体。它会在后台生成所需文档，之后可在后续练习中审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中创建待办列表。它需要准备环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看 Copilot 即将完成的任务列表。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并确定合适方案，以及编写代码。

这可能需要几分钟。云智能体的主要优势之一是能够异步执行任务，让我们专注于其他工作。接下来利用这一特点：再分配一项任务给云智能体，同时开始为应用编写其他功能代码。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分并不需要太久，这段时间也可以用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

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

稍后你会在议题的第一条评论旁看到 👀，表示 Copilot 已开始处理。

![Copilot 使用 eyes 表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在可以转而创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体异步处理，用于解决技术债务、开发功能，或将代码迁移到其他框架。

本课介绍了：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 明确界定议题范围的重要性。
- 如何为 Copilot 分配议题。

云智能体正在后台工作，现在可以开始创建并使用自定义智能体。[云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并会遵循之前模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

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
