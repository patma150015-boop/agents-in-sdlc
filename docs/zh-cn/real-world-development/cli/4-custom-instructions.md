---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "检查存储库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能 (AI) 时，上下文至关重要。如果任务需要按特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应如何组织。完成筛选功能后，你将检查 Copilot 使用的指令、添加文档规范，并将其应用于代码。

本课将：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码规范。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 对开发实践有一系列规范和要求：

- 注释应说明意图和不明显的决策，而不是重复代码内容。
- `db/` 和 `src/lib/` 中导出的函数应通过 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指引。

通过指令文件为 Copilot 提供这些信息，让它按团队实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解编码风格和要求。可以指定首选编码约定、库，甚至希望添加的注释类型。可为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：针对存储库每个请求发送的单个指令文件。内容应与大多数 Copilot 聊天或 CLI 请求相关，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他指令格式的支持各不相同。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 检查项目中的自定义指令文件

入门项目已包含一些指令文件。先检查现有内容，再进行更改并观察影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意这里包含 Astro 文件、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置一个 glob，用来确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和现有项目文件的链接。这种方式可以把较大的指令集拆分为可复用的小文件，并提供示例供 Copilot 参考。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting guidance** 部分记录了项目编码规范，但尚未要求代码内文档。接下来会添加 TSDoc 注释规范。

## 更新指令以匹配团队规范

现有文件是良好的起点，但仍有遗漏。修改核心 `copilot-instructions.md` 文件，要求新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认已检出 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何读取指令文件中的上下文，并将新规范应用到功能中。本课中，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码规范。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
