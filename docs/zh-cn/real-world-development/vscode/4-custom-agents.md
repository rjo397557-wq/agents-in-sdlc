---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业经验整理为可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护统一测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后续 Markdown 提示则定义智能体行为、专长和指令。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义智能体负责工作，技能则是工具**。

自定义智能体拥有自己的上下文窗口，能够在工作中协调技能，甚至协调其他智能体。本练习中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或测试管理技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与其他 AI 用法一样，应在目标环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

使用自定义智能体时，你将：

- 了解自定义智能体的定义方式。
- 在 Copilot Chat 中使用无障碍智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好的影响。近期用户反馈指出，部分用户认为当前深色主题难以阅读，因为文本与背景颜色对比度不足。为解决这一无障碍问题，设计团队要求增加可开关的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。接下来使用自定义智能体生成该功能。

## 查看无障碍自定义智能体

项目中已经有一个用于无障碍工作的自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 Codespace，然后查看无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 继续浏览后续部分，查看其中列出的：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，该会话会遵循相应指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每一步会在适用时说明。

1. 如果 Chat 视图的智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉菜单；默认情况下，它会显示当前智能体 **default**。
3. 从可用智能体中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并应用到项目文件。每项编辑都会以内联方式显示在聊天中，包含文件路径和供审查的差异。
6. 随着编辑进行，**Files changed** 指示器会更新，显示智能体修改的工作集。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，例如更新 Astro 组件、CSS 和相关测试。

下一练习会审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。借助自定义智能体，团队可以将专业知识和标准整理为可复用配置，引导 Copilot 更有效地完成工作。

本课介绍了：

- 自定义智能体的定义方式。
- 在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监控并引导智能体工作][next-lesson]，在同一会话中审查更改并添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
