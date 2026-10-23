---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在存储库的 `.github/agents` 文件夹中添加 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业经验固化为可复用的智能体，例如确保符合 [Web 内容无障碍指南 (WCAG)][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局存储在 `~/.copilot/agents`。每个文件都包含 YAML（YAML Ain't Markup Language）前置元数据，其中至少需要 `name` 和 `description`，后面跟着定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的比较

自定义智能体与[智能体技能][agent-skills-concept]有一些重叠之处。两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体是工作者，技能是工具**。

自定义智能体有独立的上下文窗口，可以协调技能（甚至其他智能体）完成工作。本练习中的无障碍自定义智能体会根据无障碍指南审查并更新网站；过程中可以调用拉取请求 (PR) 工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方法。与其他 AI 功能一样，需要在实际环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将介绍自定义智能体的以下用法：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于确保所有用户都能访问其众筹平台，无论用户的视觉能力或偏好如何。近期反馈指出，当前深色主题的文字与背景对比度不足，部分用户难以阅读。为解决这一无障碍问题，设计团队要求添加可由用户开关的高对比度模式。

无障碍功能至关重要，因此希望尽快实现。现在使用自定义智能体生成该功能。

## 检查无障碍自定义智能体

项目已为无障碍工作创建了自定义智能体。先检查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后打开终端，从 `main` 创建用于无障碍工作的独立分支（练习 3 的筛选 PR 会保留在单独分支中）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，查看其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - 超文本标记语言 (HTML)、层叠样式表 (CSS) 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。选择某个自定义智能体后，对话会采用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。每个练习会在需要时说明模式和模型的选择。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表。它会显示当前活动智能体，默认值为 **default**。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示词：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中显示文件路径和差异，供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示本次会话修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库中的文件，例如 Astro 组件、CSS 和相关测试。

下一练习将审查并引导这项进行中的工作。

## 总结与后续步骤

本练习探索了 GitHub Copilot 中的[自定义智能体][custom-agents]。自定义智能体是针对特定任务和领域的专业 AI 助手，可将团队专业知识和标准固化为可复用智能体，引导 Copilot 更有效地完成工作。

本练习中，你了解了：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体工作][next-lesson]，审查它提出的更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在集成开发环境 (IDE) 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
