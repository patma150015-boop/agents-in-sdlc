---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要做好准备。你将创建自己的 Tailspin Toys 存储库副本，启动一个用于练习的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 可以正常使用。

## 设置练习存储库

要为练习创建自己的代码副本，请从[模板][template-repository]创建一个实例。新实例包含练习所需的文件，后续练习都将在其中进行。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面选择 **Use this template**，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果参加由 GitHub 或 Microsoft 主办的活动，请遵循导师提供的说明。否则，可在有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新建存储库的路径（**组织或用户名/存储库名称**），后续练习会用到。

> [!NOTE]
> 待办事项已准备就绪
>
> 从模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个工作坊都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来，使用 Codespace 完成练习。

[GitHub Codespaces][codespaces] 是云端开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的开发环境，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 创建新的 Codespace。

    ![创建新的 Codespace](../../../_images/ex0-create-codespace.png)

Codespace 需要几分钟才能创建完成，但仍比手动安装所有服务快得多。等待期间，可以探索 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会使用此 Codespace。现在先让它保持打开，并留在浏览器选项卡中。

> [!NOTE]
> 本工作坊可在 Codespace 或本地[开发容器][dev-containers]中运行。两者都会安装顺利完成练习所需的先决条件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在提示时选择 **Reopen in Container**。VS Code 会构建与 Codespace 相同的开发容器。

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在集成开发环境 (IDE) 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时，该扩展通常已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可从 [Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能还需要使用 GitHub 账户进行身份验证。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息并按 Enter，以激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示登录 GitHub 账户。按提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示。

## 总结

你已创建练习存储库副本，并开始创建稍后用于编写代码的 Codespace。

## 下一步

现在开始使用 Copilot。继续学习[练习 1：自定义指令][next-lesson]，了解如何让 Copilot 遵循项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers
