---
title: "练习 3 - 使用 Playwright 模型上下文协议 (MCP) 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求前，先确认它能在浏览器中正常工作。本练习将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能，然后发布分支并创建 PR。

本练习将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区添加 Playwright MCP 服务器。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是模型上下文协议 (MCP)？

智能体模式能够访问编辑器之外的工具时，会更加强大。模型上下文协议 (MCP) 是 Copilot 实现此能力的标准方式，可让智能体与外部工具和服务通信。

![示意图显示智能体模式的内部运作，以及它如何与上下文、大型语言模型 (LLM) 和工具交互，包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为人工智能 (AI) 智能体提供与外部工具和服务通信的方式。借助 MCP，智能体可以通过资源访问最新信息，并通过工具代为执行操作。

这些工具和资源通过 MCP 服务器访问。服务器负责管理 AI 智能体与外部工具（例如现有 API 或 NPM 本地工具包）之间的通信。每个服务器都代表智能体可访问的一组工具和资源。

常见 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的应用程序编程接口 (API) 访问能力，让智能体创建存储库、更新内容、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化能力，让智能体访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表](https://github.com/mcp)，方便发现并参与生态系统建设。

> [!CAUTION]
> 安全方面，应将 MCP 服务器视为项目中的其他依赖项。使用前仔细审查源代码、验证发布者并考虑安全影响。仅使用可信的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 文件中应已为你配置 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

该 `github` 条目随项目模板提供，因此 Copilot 能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装。VS Code 通过超文本传输协议 (HTTP) 连接，并在 Copilot 首次使用其工具时让你通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控制的浏览器，正好可用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标志让 Playwright 在没有可见窗口的情况下运行浏览器，这对于没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，通常已安装 Playwright 所需的浏览器。如果 Copilot 随后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请先审查配置，然后选择信任以启动服务器。

手动启动是可选步骤。如果跳过，下一节 Copilot 首次需要其工具时，VS Code 会启动服务器并提示信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。与自行点击网站检查不同，智能体可以打开页面、导航、应用筛选条件并读取结果，然后汇报观察结果。无需离开对话即可快速确认功能行为。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是截图。智能体据此使用结构化且带标签的元素（按钮、链接、列表项）进行推理，这与辅助技术相同，因此简单的功能检查也能顺便进行基础无障碍检查。

服务器已连接且应用正在运行后，要求 Copilot 体验筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，按步骤操作并报告结果。对照议题验收条件阅读报告。如果结果有问题，可继续提问或要求它修复代码，然后再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，并审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建 PR。也可以让 Copilot 使用已连接的 GitHub 工具查找议题并关联创建 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认智能体下拉列表中选择了 **Agent**，这样 Copilot 才能使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 查找信息并执行操作。首次使用 GitHub 工具时，系统可能会提示登录 GitHub；按提示授权即可。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接审查 PR，但暂时**不要合并**。

## 总结与后续步骤

本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 提供新工具。
- 在工作区添加了 Playwright MCP 服务器。
- 使用该服务器在发布前通过浏览器手动测试筛选功能。
- 发布分支并为筛选功能创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
