---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你已经：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在计划模式中确定方案，在自动驾驶模式中构建，然后在交互模式中审查。
- 使用自定义说明引导智能体，随后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的更改

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你一直使用同一个对话和分支。为简化本工作坊，这些工作合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 使用 Copilot 探索浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体结合这些结果，在 PR 创建前检查了需求和测试覆盖范围。

新增测试应解决实际缺口；QA 检查不需要新增测试也可能是正确的。授权合并前，请检查代码和验证证据。更改后，应重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个会话中复用这些自定义项，并根据团队需求调整。说明用于设定标准，技能用于描述可重复的任务，自定义智能体用于定义专用角色，MCP 服务器则用于连接外部工具。请检查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用**计划**模式在构建前梳理方案，使用**交互**模式紧跟聚焦更改的进展，并使用**自动驾驶**模式处理范围明确的任务。例行编辑可以选择速度更快的模型；复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出。

## 深入探索

核心工作流已完成。还可以继续了解一些 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获取不同视角。
- 使用 `/fleet` 并行协调互相独立的子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

持续使用工具是提升技能的最佳方式。可以将它用于生产代码、个人项目，或用于实现想了很久却一直没时间做的小应用。与团队分享经验，也向团队成员学习。最后，别忘了继续探索文档。

若要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。若想比较其他工作环境，请浏览 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
