---
title: "第 6 课 - 使用 Playwright 模型上下文协议 (MCP) 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码并不是开发工作的全部。我们还需要处理数据、外部服务，并为 Copilot 提供更多自动化能力。MCP 服务器可以让 Copilot CLI 使用内置功能以外的工具和服务。

本课将：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体操作浏览器，体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证用户界面 (UI) 更新也需要实际操作界面。通过 Playwright MCP，Copilot 可以像用户一样使用正在开发的网站，进一步自动化更改验证，并提高对功能行为的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为人工智能 (AI) 智能体提供了与外部工具和服务通信的方式。智能体借助 MCP 可以实时访问最新信息并代为执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁；每个 MCP 服务器都代表智能体可访问的一组工具和资源。

常见 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的应用程序编程接口 (API) 访问能力。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，方便发现并参与生态系统建设。

> [!CAUTION]
> 将 MCP 服务器视为项目中的其他依赖项。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已经配置 Playwright MCP，请跳过设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入服务器名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前已手动确认功能正常。现在使用 Copilot 中的 Playwright MCP 服务器执行相同验证。

1. 使用以下提示词，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，它会自行判断。不过，明确指出工具有助于稳定结果，并减少额外尝试。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 操作网站、停止服务器并提供报告。

## 总结与后续步骤

你已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器体验功能。本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加了该服务器。
- 让智能体操作浏览器并体验了筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专业角色中。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
