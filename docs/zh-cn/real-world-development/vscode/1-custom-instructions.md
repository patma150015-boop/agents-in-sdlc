---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将：

- 了解存储库自定义指令和按路径限定的指令文件如何向 Copilot 提供项目上下文、编码指南和文档规范。
- 在当前指令下，生成筛选功能的第一个数据层切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。
- 再次运行相同提示词，观察生成代码如何遵循新规范。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不会遵循所有规范。Copilot 的行为具有概率性。本练习旨在观察更新指令后行为的整体变化，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一系列开发实践规范和要求：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

通过指令文件，可以向 Copilot 提供这些信息，帮助它按照团队实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定编码约定、库，以及希望添加的注释类型。既可以为整个存储库创建指令，也可以为特定文件类型或任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：针对存储库的每个请求发送的单个指令文件。内容应是大多数 Copilot 请求都适用的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在集成开发环境 (IDE) 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都可以使用存储库级指令，以及带 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持 AGENTS.md 和 CLAUDE.md 等其他规范的指令文件，详情见[自定义指令支持][custom-instructions-support]。

### 管理指令文件的最佳实践

创建指令文件的完整讨论超出本工作坊范围。示例项目展示了一种常见方法：

- 在 `copilot-instructions.md` 中保留项目级指导，例如项目说明、结构和全局编码规范。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件、数据层）或任务提供指引。
- 使用自然语言，确保指导清楚，并提供代码示例说明正确和错误的写法。

创建指令文件或使用 AI 没有唯一的方法。通过试验找出最适合项目的方式。

> [!TIP]
> 使用 GitHub Copilot 的项目都应具备完善的指令文件。在本项目的文件中，可以找到针对 [UI 更新][ui-instructions]、[Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮你生成指令文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应练习会说明适用方式。
>
> 如需模板或起点，可查看包含指令文件、自定义智能体及其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览项目中的自定义指令文件

先阅读项目随附的指令文件，包括核心 `copilot-instructions.md` 和用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引。这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它会设置相对于存储库根目录的 glob。任何匹配的 TypeScript 测试文件（例如 `**/*.test.ts`）都适用。
5. 查看该文件中针对本项目单元测试的指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看末尾。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的指令集拆分为可复用的小文件，并提供示例供 Copilot 参考。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头规范。

## 为更改创建分支

先为本次更改创建分支。

1. 返回上一练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。每个练习会在需要时说明模式和模型的选择。

先在当前指令下发送提示词，之后更新指令并重新发送相同提示词，观察变化。

1. 关闭上个练习中打开的编辑器选项卡，避免 Copilot 获得不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应位于何处。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 可以应用文件更改。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时更改辅助函数文件和测试。
6. 留意建议的辅助函数是类型化函数，第一个参数为 `db` 客户端，并返回类型化的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的行为具有概率性，即使未明确要求，也可能添加文档注释。即使发生这种情况也没关系；本练习重点在于更新指令后行为的一致性提升。

## 添加新的存储库规范

如前所述，`.github/copilot-instructions.md` 用于提供项目级信息。现在添加一项全局规范，改善 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。留意这里记录了项目编码规范，但尚未包含代码内文档要求。
3. 在现有规范下方添加以下 Markdown 条目，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可以放在存储库级别的 `.github/copilot-instructions.md` 中，为所有任务提供指导，也可以使用 `*.instructions.md` 文件限定到特定语言、文件类型或任务。刚添加的文档注释规范属于项目级标准，适合放在存储库级文件中。

## 重新发送提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用于下一练习会继续使用的实际代码。

## 提交指令并推送分支

指令文件和其他项目内容一样，通过源代码管理进行维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新及 Copilot 为辅助函数基础所做的更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也请将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送。接下来创建拉取请求，并将其关联到待办事项中的文档规范议题。也可以让 Copilot 使用已连接的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按提示授权即可。后续练习会介绍此连接所使用的模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中了 **Agent**。
3. 要求 Copilot 查找更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示 **Allow** 它代表你执行 GitHub 操作，请审查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示词要求 Copilot 将 PR 合并到 `main`，并在本地切回 `main` 拉取最新代码：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示 **Allow** 它执行 GitHub 操作和 shell 命令，请审查命令并酌情批准。

你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你了解了 Copilot 如何读取指令文件中的上下文，并在 VS Code 中：

- 发送代码生成提示词，观察 Copilot 根据现有指令生成的内容。
- 在 `.github/copilot-instructions.md` 中添加存储库级规范。
- 再次发送相同提示词，观察生成代码如何遵循新规范。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中应用这些指令，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
