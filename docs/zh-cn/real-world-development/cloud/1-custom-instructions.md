---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅理解要做*什么*，也理解应当*如何*完成。

本练习将：

- 了解存储库自定义指令和按路径限定的指令文件如何向 Copilot 提供项目上下文、编码指南和文档规范。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 工作环境不同，本练习不进行前后提示词对比。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示影响。后续练习审查云智能体创建的拉取请求时，可以看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 有一系列开发实践指南和要求：

- 数据层始终需要单元测试。
- 用户界面 (UI) 应采用深色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

通过指令文件为 Copilot 提供这些信息，帮助它遵循团队实践。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定编码约定、库，以及希望添加的注释类型。既可以为整个存储库创建指令，也可以为特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：针对存储库的每个请求发送的单个指令文件。内容应是大多数 Copilot 请求都适用的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：针对特定任务或文件类型提供指导，例如 TypeScript、Astro、UI 组件或单元测试。

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

花些时间阅读存储库随附的指令文件：一个核心 `copilot-instructions.md` 和一组用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指引。这些内容适用于 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它会设置相对于存储库根目录的 glob。任何匹配的 TypeScript 测试文件（例如 `**/*.test.ts`）都适用。
5. 查看该文件中针对本项目单元测试的指引。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的指令集拆分为可复用的小文件，并提供示例供 Copilot 参考。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求代码内文档。接下来会添加 TSDoc 注释和文件注释头规范。

## 添加新的存储库规范

接下来编辑文件，添加项目级规则，要求代码中包含 TSDoc 文档注释和文件级注释头。之后的练习中，云智能体处理议题时会读取这项规则。

编辑前先创建分支（云智能体会读取目标议题分支上的指令，但这里仍会按规范提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于提供项目级信息。现在添加编码规范，改善 Copilot 的建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，位置大约在第 27 行。留意这里记录了项目编码规范，但尚未要求代码内文档。
3. 在现有规范下方添加以下 Markdown 条目，要求 Copilot 添加文件注释头和 TSDoc 注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件既可以放在存储库级别的 `.github/copilot-instructions.md` 中，为所有任务提供指导，也可以使用 `*.instructions.md` 文件限定到特定语言、文件类型或任务。刚添加的文档注释规范属于项目级标准，适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习将把议题分配给 Copilot；它会从 `main` 创建分支，因此必须先将指令更改合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上从 `custom-instructions-cloud` 创建到 `main` 的拉取请求并合并。之后的练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本工作坊中直接操作 `main`，可以跳过分支并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何读取指令文件中的上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级规范。后续工作环境中的拉取请求会体现这项规范。

接下来，将第一个议题[分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
