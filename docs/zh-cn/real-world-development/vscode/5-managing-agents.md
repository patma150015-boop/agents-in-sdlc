---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 GitHub Copilot 承担了三项改善用户体验和增加功能的任务。智能体模式会自主探索代码库、提出更改、编辑文件并运行命令。由于 Copilot Chat 在 VS Code 中实时工作，你可以查看每次工具调用和文件编辑，逐项审查差异、接受或拒绝更改，并通过后续提示词引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体添加浅色模式开关，并保留高对比度模式。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作尚未完成时，设计团队又提出了浅色模式需求，以便偏好较亮主题的用户使用。与其重新开始对话，不如引导当前会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体采取的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中内嵌显示差异以及 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时滚动查看聊天，跟踪每次工具调用，包括文件读取、编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内嵌显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看本次会话期间智能体修改的全部文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。仅批准你认可的命令。

5. 等待智能体完成高对比度模式。完成后，它会显示更改摘要。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在在同一对话中扩展工作，添加浅色模式开关。继续使用同一聊天会话时，智能体会保留之前工作的上下文，包括涉及的组件、采用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 智能体会检查已有更改，并提出添加浅色模式的方案。
3. 和高对比度模式一样，逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的方向不符合预期（例如尝试以不需要的方式重构开关用户界面 (UI)），可选择聊天输入框旁的 **Stop** 按钮停止，然后发送补充说明。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以通过后续提示词要求调整（“简化层叠样式表 (CSS)，使用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个开关，将浅色模式整合到现有设置面板”）或扩展范围（“为两个开关添加键盘快捷键”）。

## 审查工作集

提交前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查更改文件列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个更改文件并检查差异。确认自定义智能体提出的无障碍模式已落实，包括无障碍富互联网应用 (ARIA) 属性、键盘导航、语义化超文本标记语言 (HTML) 和本地存储持久化。
4. 确认更改后，在 Source Control 面板中暂存并提交。下一课将发布这些更改。

## 总结与后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在进行的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示词扩展工作而不丢失上下文。

本练习中，你：

- 实时监控了智能体工作。
- 接受或拒绝了单项文件编辑。
- 通过后续提示词在会话进行期间引导智能体。

下一课将发布无障碍更改并创建拉取请求，同时审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
