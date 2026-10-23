---
title: "第 7 课 - 创建并使用质量保证 (QA) 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright 模型上下文协议 (MCP) 进行验证。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并通过 Playwright 模型上下文协议 (MCP) 在浏览器中观察筛选功能。现在将这些能力整合到具有明确质量保证 (QA) 流程的自定义智能体中。

本课将：

- 了解自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，一致地审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专业版本。配置文件描述智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用过的自定义功能各有用途：存储库指令说明团队规范；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告结果。QA 配置文件不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。该文件将明确 QA 要执行的检查和必须遵守的界限。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 要求默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查配置文件，确认 Copilot 准确记录了预期的 QA 流程和边界。这样可以避免配置不完整或范围过大的智能体在只需验证时修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但添加后会有清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、在修改实现代码前征求许可，并且不会提交更改或创建拉取请求。
5. 如果配置文件缺少任何职责或限制，请先要求默认智能体修订，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一个筛选功能对话并选择 QA，使其能够使用已有的议题和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能尚未显示在可用智能体列表中，但可以通过上述命令启用。

2. 使用以下提示词要求 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成工作。
4. 阅读它提供的报告。

## 总结与后续步骤

你为工作流添加了可复用的专业角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何结合指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查了结果。

现在，实现、技能更新、QA 配置文件、测试和验证报告均已准备好接受审查。接下来，[将它们整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
