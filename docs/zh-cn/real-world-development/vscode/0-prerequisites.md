---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本，启动一个用于学习的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置练习存储库

要创建用于练习的代码副本，需要从[模板][template-repository]创建一个实例。新实例会包含练习所需的所有文件，后续练习都会使用它。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面中选择 **Use this template** 按钮，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果是在 GitHub 或 Microsoft 举办的活动中参加本工作坊，请按照导师提供的说明操作。否则，可以在有权使用 GitHub Copilot 的组织中创建新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下所创建存储库的路径（**组织或用户名/存储库名称**），后续练习会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，系统会自动为你创建一组 GitHub 议题作为待办事项。整个工作坊都会使用这些议题，无需自行创建。

## 创建 codespace

接下来，使用 codespace 完成本练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能，接下来我们会介绍这些功能。

> [!CAUTION]
> 后续练习还会用到这个 codespace。现在先不要关闭它，请在浏览器标签页中保持打开。

> [!NOTE]
> 本工作坊可在 codespace 或本地 [dev container][dev-containers] 中运行。这两种环境都已安装顺利完成练习所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时，通常已预装该扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 Enter。这样即可激活 Copilot Chat。
4. 如果尚未完成身份验证，系统会提示登录 GitHub 账户。按照提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口就会显示出来。

## 总结

恭喜你，已创建练习存储库的副本，并开始创建后续编写代码时要使用的 codespace。

## 下一步

现在开始让 Copilot 帮忙。继续学习[练习 1：自定义说明][next-lesson]，向 Copilot 介绍项目规范。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
