---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的人工智能 (AI) 助手。通过在存储库的 `.github/agents` 文件夹中添加 Markdown 文件，可以为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定工作。团队可以将专业知识固化为可复用智能体，例如确保符合 [Web 内容无障碍指南 (WCAG)][wcag] 的无障碍智能体、遵循安全编码实践的安全智能体，或保持测试模式一致的测试智能体。

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

返回 Codespace，然后检查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，查看其中介绍的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误清单。

## 创建并分配议题

任务控制中心 (Mission control) 是管理环境中所有智能体的中心位置。可以在此将任务分配给 Copilot 云智能体、监控任务，或重新引导并补充指导。现在把创建高对比度模式的任务分配给 Copilot。

1. 打开自己的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 在 **Title** 中输入 `Add high contrast mode to website`。
6. 在 **Description** 中输入：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

   ![云智能体分配截图，突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结与后续步骤

本练习介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即针对特定任务和领域的专业 AI 助手。自定义智能体可将团队专业知识和标准固化为可复用智能体，引导 Copilot 更有效地完成工作。

本练习中，你了解了：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，在任务控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为在组织中使用自定义智能体做准备][org-custom-agents]
- [为在企业中使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
