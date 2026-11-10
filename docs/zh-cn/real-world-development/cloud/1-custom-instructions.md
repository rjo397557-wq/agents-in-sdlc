---
title: "练习 1 - 自定义说明（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能获取这些上下文。[说明文件][instruction-files]用于提供此类指导，让 Copilot 不仅了解要做*什么*，也了解应该*如何*完成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流不同，本练习不会进行提示的“前后”对比。Copilot 云智能体会异步处理 GitHub 议题，难以实时并排展示效果。之后审查云智能体创建的拉取请求时，可以看到说明文件的影响。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件顶部都应有注释块，说明文件用途。

使用说明文件可以确保 Copilot 掌握正确信息，并按照这些实践执行任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定偏好的编码约定、库以及希望加入的注释类型。可以为整个存储库或特定文件类型创建说明，分别提供项目级或任务级上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单一说明文件。它应包含对大多数聊天或 CLI 请求都有用的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或为创建 UI 组件、增加单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中工作时，说明文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级说明文件以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还支持其他标准的说明文件，例如 `AGENTS.md` 和 `CLAUDE.md`。[查看自定义说明支持情况][custom-instructions-support]。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出本工作坊范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 中的说明聚焦于项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

创建说明文件没有唯一正确的方法，使用 AI 也是如此。通过实验找到最适合项目的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的说明文件。浏览本项目文件时，可以看到其中包含 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指导。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本工作坊会在相关课程中介绍具体入口。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中汇集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

本存储库已经包含一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在 GitHub Web UI 中打开，也可以在已启动的 Codespace 中查看：

- 打开 `.github/copilot-instructions.md`，查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下嵌套的 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的任何交互。
- 浏览 `.github/instructions` 文件夹，查看针对 Astro 文件、Drizzle 数据层、测试等内容的说明。
- 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它设置了相对于存储库根目录的 glob，决定说明适用范围，例如 `**/*.test.ts`。
- 打开 `.github/instructions/drizzle.instructions.md`，查看指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。相关路径是相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `.github/copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库级标准

这是本练习唯一需要编辑文件的步骤：添加一项项目级规则，要求代码中包含 TSDoc 文档注释和文件级注释头。后续练习中，云智能体会在处理你分配的议题时读取该规则。

编辑前先创建分支。云智能体会读取议题目标分支中的说明，但仍应将这项更改清晰地提交：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在为存储库记录编码标准，以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。注意其中记录了项目编码标准，但还没有代码文档规则，这正是生成的 helper 缺少文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别创建说明文件（`.github/copilot-instructions.md`）提供全局指导，也可以创建 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚才添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

云智能体会读取议题目标分支中的说明。下一练习中，Copilot 会从 `main` 分支创建分支，因此需要先将说明更改合并到 `main`。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。之后为 Copilot 分配议题时，云智能体便会读取这些说明。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处采用分支，是为了模拟真实项目的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从项目中的说明文件获取上下文，并向 `.github/copilot-instructions.md` 添加了存储库级标准。接下来几课中，可以观察该标准如何影响云智能体创建的拉取请求。

下一步，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
