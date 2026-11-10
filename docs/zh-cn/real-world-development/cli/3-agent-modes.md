---
title: "课程 3 - 智能体模式：计划和自动驾驶"
description: "使用计划模式确定方案、使用自动驾驶模式根据议题实现筛选功能，并在交互模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的更改则需要更完整的流程。GitHub Copilot CLI 可以配合组织现有的开发流程，帮助我们用合适的方式构建正确的功能。接下来几课将介绍典型的智能体辅助开发流程：从议题出发实现功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续完成此功能的过程中，会继续使用同一个对话和分支。通常会针对不同文件类型创建不同分支或 PR，但本工作坊会简化流程，以便专注于核心概念。

本课将：

- 根据 GitHub 议题启动新的 Copilot 对话。
- 在计划模式中确定需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续还会更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了这项功能，但在编码前，需要先确定如何组合多个类别等细节。你将使用计划模式确定这些方案，然后授权自动驾驶模式在明确的范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则会变得更加重要。大多数开发人员都会遵循类似的流程：

1. 从描述需求的已登记议题开始。
2. 规划要构建的内容。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码符合既定要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，但无法编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并迭代，直到任务完成。

先使用计划模式并审查计划，然后再使用自动驾驶模式实现功能。

## 从议题开始

开始筛选功能的工作前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示，然后返回 **Session** 选项卡。

此时，提示前面会显示 `#7` 或类似编号。通过 `#` 可以将 GitHub 上的议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以让你在交给 Copilot 实现之前，确定功能的实现方式或任务的执行方法。遇到复杂任务时，花些时间规划总是值得的。现在切换到计划模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划。它会先探索项目，再确定适当的实现方案。

3. Copilot 可能会询问筛选功能的工作方式。根据自己的偏好回答，没有标准答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何内容与你确定的方案不符，请要求 Copilot 修改。

## 批准自动驾驶模式

计划已经编写并审查完毕，现在开始实现。使用自动驾驶模式让 Copilot 执行工作。

自动驾驶模式会让 Copilot 迭代解决问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按照既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再确认检查已完成。

## 审查并验证实现

与其他代码一样，生成的代码在合并前也需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 对照议题和规划时确定的方案检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并要求 Copilot 修复失败项。

## 试用新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用，然后通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置行为和空结果行为符合议题要求及已确定的方案。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你已经：

- 根据 GitHub 议题启动了新的 Copilot 对话。
- 在计划模式中确定需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，通过[使用自定义说明][next-lesson]深入了解代码生成，确保代码遵循记录在案的规范。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
