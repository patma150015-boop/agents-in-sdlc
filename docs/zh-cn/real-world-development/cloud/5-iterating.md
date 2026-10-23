---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本工作坊中，你让 GitHub Copilot 执行了多项改善用户体验和增加功能的任务：为代码添加文档、为设计团队构建相关游戏功能，以及实现高对比度和浅色模式开关等无障碍功能。现在检查代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。仍需审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用审查前，先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## GitHub Copilot 云智能体的安全性

Copilot 云智能体异步且无需持续监督地执行任务，因此设置了一些安全限制：

- Copilot 对存储库只有读取权限，并且仅对用于编写代码的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在单独的临时环境中工作。
- GitHub Actions 工作流必须由人工批准后才能运行。
- [默认限制外部资源访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议 (MCP) 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个 PR，即为代码添加文档的 PR。使用 GitHub.com 上的标准 PR 界面审查。

> [!NOTE]
> 查看拉取请求 (PR) 时，可能会看到提示 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部模型上下文协议 (MCP) 服务器。如果需要，可[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待 Copilot 完成。可能需要几分钟，可以稍作休息或回顾已学内容。

4. PR 准备好后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

   检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自练习 2 中审查的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会加入 PR 的检查列表。正常情况下，单个 Astro 应用的项目检查会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。可以像提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动另一个会话处理请求。由于结果具有概率性，无法提供固定措辞；可以要求 Copilot：

- 在每个代码文件顶部添加简短说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 在评论中提及 **@copilot**，要求它修改生成的文档。可使用上述建议，也可以提出其他与代码文档相关的要求。
2. 选择 **View Session**，查看 Copilot 如何处理更改。注意它会启动新会话。
3. 选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意为止。之后可将 PR 从草稿状态改为准备审查，并合并到主分支。

![将 PR 转为准备审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为解决游戏详情页相关游戏议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会加入 PR 的检查列表。正常情况下，单个 Astro 应用的项目检查会通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 中的终端，运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。和之前一样，可以通过评论迭代并请求更新，例如调整显示的相关游戏数量，或补充注释头和 TSDoc 注释（请注意，分配该任务时还未更新自定义指令）。在 **Conversation** 选项卡中添加评论后，Copilot 会读取请求并启动新会话。

## 审查无障碍功能

最后审查使用无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中要求的高对比度模式，以及练习 4 在任务控制中心追加要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡并审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的层叠样式表 (CSS) 或样式更改。
   - 无障碍富互联网应用 (ARIA) 标签、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会加入 PR 的检查列表。正常情况下，单个 Astro 应用的项目检查会通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。在 Codespace 中打开终端，并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认设置在重新加载页面后仍然有效。

注意无障碍自定义智能体如何引导 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或改进空间，可以像之前一样在评论中提及 **@copilot** 并要求更新。

## 可选练习 — 继续委派任务

云智能体适合处理真实待办事项，让你有时间专注其他工作。可以为存储库创建一些议题并分配给 Copilot，练习这一工作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体工作环境。本系列练习中，你：

- **检查了存储库自带的自定义指令文件**，以便稍后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、规划和异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代云智能体的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体工作环境的练习已完成。其他工作环境可以从不同角度补充本次实践：

- 🖥️ **[VS Code 工作环境](../../vscode/)** — 直接在集成开发环境 (IDE) 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作环境](../../cli/)** — 使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体连接，从终端交付两个经审查的更改。

也可以在自己的存储库中尝试以下后续事项：

- 将重构或测试覆盖议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈配置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
