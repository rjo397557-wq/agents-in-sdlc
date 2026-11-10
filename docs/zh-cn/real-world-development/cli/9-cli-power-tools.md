---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选委派给云智能体的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

和其他优秀的 CLI 工具一样，GitHub Copilot CLI 也提供许多斜杠命令。它们可以访问高级功能、查看后台信息并配置其他选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来探索其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树以及委派给云智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在探索一些其他功能：管理上下文、切换模型、共享会话，以及选择是否将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以通过斜杠命令自行查看或压缩上下文。

1. 返回 Codespace；如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文在系统说明、工具、消息和可用空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。切换到无关任务并希望开启全新对话时，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有不同偏好。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员分享经验可以帮助大家改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布导出内容前，请先检查内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心工作坊已完成。Copilot CLI 还为更大规模的任务提供了以下命令：

- `/fleet` 可以把互相独立的子任务分配给多个子智能体并行处理。
- `/worktree` 可以为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给异步工作的 Copilot 云智能体；它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。试用前，请先创建范围明确的新任务，并按常规工作流审查结果。若要进一步了解异步智能体工作，请继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并查看后台状态。本课中，你已经：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树以及委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令值得探索。现在通过[回顾已学内容][next-lesson]并了解后续步骤，为本次学习画上句号。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
