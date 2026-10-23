---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要更改多个文件并执行测试等操作。开发人员需要找到所有相关文件、进行更改、运行测试、调试、查找遗漏的文件，再继续修改，工作往往如此反复。

这正是 Copilot 智能体模式的用武之地。

Copilot 智能体模式可以在集成开发环境 (IDE) 中更自主地工作。它会先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在体验如何使用智能体模式为网站添加功能。

> [!NOTE]
> 尽管名称相似，智能体模式和云智能体面向两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增加，希望访客能按类别和发行商筛选。上一练习已添加发行商辅助函数，现在使用 Copilot 智能体模式完成其余数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改前先查看 Tailspin Toys 网站，了解当前功能。

该网站是面向开发者的桌游众筹平台，可列出游戏并显示详情。它是一个 Astro 应用，会在构建时将页面渲染为静态超文本标记语言 (HTML)。页面通过 Drizzle 对象关系映射 (ORM) 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端应用程序编程接口 (API) 或客户端用户界面 (UI) 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能使用少量带作用域的 Astro `<script>` 和标准文档对象模型 (DOM) API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后，你应该已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新的终端。
3. 创建并切换到用于筛选功能的分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果对话框提示为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost 链接后，系统会自动将其重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，此时 Codespace 正在托管 Web 服务器。

### 浏览网站

网站运行后，可以浏览其功能：

- **主页**：显示桌游列表及其标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、说明、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。每个练习会在需要时说明模式和模型的选择。

网站的初始实现已经可以使用，但仍有改进空间。先查看待办事项。通过模板创建项目时，系统已自动生成议题；请让 GitHub Copilot 展示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。它可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者不偏好某一种模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将其列入说明，以便尽量保持建议的一致性。但由于大型语言模型 (LLM) 具有概率性，收到不同建议属于正常现象。

> [!NOTE]
> 由于 LLM 的概率特性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示词，询问存储库待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，允许运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还根据对用户的价值为事项安排了优先级。

## 审查指令文件

让智能体生成代码前，先查看用于提供工作上下文的指令文件。本练习将使用[用户界面 (UI)][ui-instructions]文件，其中包含向网站添加功能的指导。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向 Astro、Tailwind CSS 和 Drizzle 等文件类型专用指令的链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指引、拆分复杂指令并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新应用的三个部分：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，添加筛选用户界面 (UI)。

合并前还必须运行并通过测试。让 Copilot 智能体模式完成这些任务。

1. 可继续当前 Copilot 对话，也可选择 **New Chat** 新建对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选择了 **Agent**。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办事项中相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并定位相关文件。它应找到数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试，可能需要几分钟，具体取决于任务复杂度和代码库。在此过程中，编辑器可能显示 **Keep** 和 **Undo** 按钮。Copilot 完成前无需选择它们；完成后可针对所有更改选择保留或撤销。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示用户确认运行测试的截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停或出现测试失败，这属于正常情况。Copilot 会在代码生成和测试之间反复迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式完整聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他人工智能 (AI) 工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。本练习中，你了解了如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能。
- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 合并代码前审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为筛选功能创建拉取请求。

### 附加探索练习 - 实现分页

随着游戏列表不断增加，网站需要支持分页。使用本练习学到的技能，让 Copilot 更新网站以实现分页。编写提示词时可考虑：

- 遵循现有最佳实践，包括使用项目现有指令文件。
- 决定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 提供足够的上下文，让 Copilot 按预期实现。
- 可能需要通过迭代、提出更改请求并补充上下文来完善结果。这是使用 Copilot 的正常流程。

## 资源

- [Copilot ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
