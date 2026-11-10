---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "查看存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅描述想要编写*什么*代码，也描述代码应当*如何*组织。完成筛选功能后，现在可以了解 Copilot 使用了哪些说明、添加一项文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

和优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 应保留现有的格式和 lint 指导。

使用说明文件可以确保 Copilot 掌握正确的信息，并遵循这些实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定偏好的编码约定、库以及希望加入的注释类型。说明可以覆盖整个存储库，也可以仅针对特定文件类型。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单一说明文件，其中应包含对大多数请求都有用的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 探索本项目中的自定义说明文件

起始项目中已包含一组说明文件。先查看现有内容，再进行更改，以便了解更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于与 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了决定适用文件范围的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它链接到的其他说明文件和项目文件。这样可以将较大的说明集拆分成更小、可复用的文件，并为 Copilot 指出可参考的示例。

## 更新说明文件以符合团队指导

现有文件是不错的起点，但仍缺少一项指导。修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分的最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，这样 Copilot 就能在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改过的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。具体来说，你已经：

- 了解存储库说明和按路径限定范围的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
