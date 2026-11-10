---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就应确保它能获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[说明文件][instruction-files]来指导代码生成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在当前说明生效的情况下，生成筛选功能的第一块数据切片（publishers helper）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能基础更改提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能与部分标准不完全一致。Copilot 具有概率性而非确定性。本练习要观察的是更新说明后行为变化的*趋势*，而不是要求输出逐字符一致。

## 说明文件

### 场景

和其他优秀的开发团队一样，Tailspin Toys 也有一套开发实践准则和要求：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件顶部都应有注释块，说明文件用途。

使用说明文件可以确保 Copilot 掌握正确信息，并按照这些实践执行任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定偏好的编码约定、库以及希望加入的注释类型。可以为整个存储库创建说明，也可以针对特定文件类型创建说明，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单一说明文件。它应包含项目级信息，即大多数聊天或 CLI 请求都会用到的上下文，例如技术栈、项目概况、最佳实践和全局指导。
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

花些时间阅读本存储库自带的说明文件：一个核心 `copilot-instructions.md` 和一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的嵌套 **GitHub Actions Workflows** 指导。这些说明适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览，查看针对 Astro、Drizzle 数据层、测试等内容的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了相对于存储库根目录的 glob，用来决定说明适用的文件。在此例中，匹配 `**/*.test.ts` 的 TypeScript 测试文件都适用。
5. 查看本项目中用于创建单元测试的专门说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它链接到其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件。这样可以将较大的说明集拆分成较小、可复用的文件，并为 Copilot 指出可参考的示例。相关路径是相对于说明文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中编写文档。接下来的步骤会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循最佳实践，为本次更改创建分支。

1. 返回上一练习使用的 Codespace。
2. 打开新终端，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型会因练习而异；每一步会在适用时说明。

为了观察自定义说明的影响，先在当前说明生效时发送提示。稍后会更新说明，再次发送相同提示进行比较。

1. 关闭之前练习遗留的编辑器标签页，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意生成的 helper 是类型化函数，第一个参数是 `db` 客户端，返回类型化的 publishers 数组。这遵循 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使没有明确要求，也可能添加文档注释。如果出现这种情况也没关系；这里要观察的是更新说明后*一致性*的提升。

## 添加新的存储库级标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，以改善代码建议。

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

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的 publisher 数据访问辅助函数。
    * 提供从数据库检索 publisher 信息的函数。
    */
   ```

5. 注意生成的函数现在会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回所有 publisher 的列表，包含其 id 和 name。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 一个 Promise，解析为 publisher 对象数组。
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习会继续使用的真实代码。

## 提交说明并推送分支

说明文件与其他项目文件一样，通过源代码管理进行维护。现在将更改提交并推送到存储库。

1. 打开新终端，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
2. 在终端中运行以下命令，确认说明和 helper 更改已经存在：

   ```bash
   git status
   ```

3. 暂存并提交说明更新以及 Copilot 为 helper 基础更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

将分支推送后，创建拉取请求并关联待办事项中的文档标准议题。也可以请 Copilot 查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是 Copilot 首次代表你在 GitHub 上执行操作。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接所使用的模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明说明更新和新 publishers helper 基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. Copilot 请求你 **Allow** GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你 **Allow** GitHub 操作和运行 shell 命令时，检查命令并酌情选择 **Allow**。

现在已经借助 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你已了解 Copilot 如何从项目中的说明文件获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 如何遵循*现有*说明。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和 helper 基础更改提交到 `main`，供下一练习使用。

接下来，使用[智能体模式][next-lesson]让 Copilot 将说明应用于跨代码库的功能更改。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot——说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
