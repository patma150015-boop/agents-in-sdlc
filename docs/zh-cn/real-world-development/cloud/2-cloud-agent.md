---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都需要处理技术债务，例如尚未解决的问题、需要更新的旧代码，或因时间不足而搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。任务完成后，它会创建草稿拉取请求 (PR)，供开发人员审查。这样可以把繁琐工作交给智能体，让开发人员专注于更重要的事项。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 了解如何明确界定议题范围。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包团队没有留下完善文档，实际上几乎完全缺失。团队希望先为应用中所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们想在每款游戏的详情页上显示相关游戏，即相同类别的其他游戏，让访客可以继续浏览。目前不需要完善的实现，只希望有一个可用于用户体验验收测试的版本。此项工作目前受阻，但还有优先级更高的议题。

这些任务很容易被推迟，适合分配给 Copilot 云智能体。它可以异步处理任务，让开发人员先做其他工作，之后再审查结果并确认是否符合预期。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。可以通过多种方式交办工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始执行任务。任务进行期间或完成后，都可以查看 Copilot 会话。解决方案准备好审查时，Copilot 会在拉取请求中通知你。

## 明确界定任务范围的重要性

GitHub Copilot 没有魔法，不能代替思考。看似简单的操作也可能包含复杂性，因此应[谨慎界定交给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作人工智能 (AI) 结对编程助手：分阶段工作、学习、试验并逐步调整。加入生成式人工智能不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已完成自定义指令练习，就已经看到 `.github/copilot-instructions.md` 的作用；否则，现在可以快速查看。将工作分配给 Copilot 云智能体前，先只读检查存储库现有指令文件，以便稍后观察其影响。

在存储库的 GitHub 网页界面中打开以下文件；如果已有 Codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，特别是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的 PR 中是否出现 TSDoc 注释、TypeScript 约定和文件注释头。这些要求来自指令文件，后续练习审查 PR 时还会再次讨论。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本，确保一切就绪。给 Copilot 分配任务时也一样；它会以类似软件工程师 (SWE) 的方式工作。

云智能体使用 [GitHub Actions][github-actions] 环境执行工作。可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]来自定义环境，并在智能体开始工作前安装所需开发工具和依赖项。练习已预先配置此工作流，以便顺利完成活动并展示相关功能。它会为单个 Astro 应用准备 Node.js、项目依赖项、端到端测试所需的浏览器，以及已迁移并植入数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有数据可用。
      - name: Set up the database
        run: npm run db:setup
```

这看起来像普通 GitHub 工作流文件，但有几点值得注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会执行该作业。
- `workflow_dispatch` 触发器允许在存储库的 **Actions** 选项卡中手动运行工作流。这样可以先确认工作流正常，而不必等 Copilot 调用它。

## 添加文档

大多数项目都存在信息过时或文档缺失的问题。这类技术债务往往长期未处理，会降低效率、增加代码库维护难度，也让新成员更难加入。Copilot 擅长创建文档，因此适合将这项工作分配给云智能体。它会在后台生成所需文档，之后可以回来审查。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的 PR，其标题类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请等待一两分钟并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办清单。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办清单，因为它需要创建环境（运行前面提到的工作流）、分析项目并确定解决问题的方案。

13. 查看清单及其将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面，才能看到更新后的状态指示器。

16. 滚动查看实时会话，了解 Copilot 如何解决问题，包括探索代码、了解现状、暂停思考并制定方案，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一就是异步处理任务，让你可以专注其他工作。现在利用这一功能，再分配一项任务给云智能体，然后转而为应用编写其他功能。

## 为游戏详情页添加相关游戏

GitHub Copilot 云智能体的一个优势是可以拆分工作：你处理一组任务，它处理另一组任务。为设计团队添加相关游戏区域可能不会花太久，但这段时间仍可用于其他工作。现在把任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 在 **Title** 中输入 `Show related games on the game details page`。
6. 在 **Description** 中输入：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中应会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正处理第二个请求。云智能体的工作方式类似软件工程师，因此无需持续监控；等它完成后再审查即可。现在继续了解如何创建和使用自定义智能体。

## 总结与后续步骤

本练习介绍了 [GitHub Copilot 云智能体][copilot-agents]。使用云智能体可以异步分配议题，让 Copilot 处理技术债务、开发新功能，或协助将代码迁移到其他框架。

本练习中，你了解了：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以创建并使用自定义智能体。[Copilot 云智能体也可以使用模型上下文协议 (MCP) 服务器][cloud-agent-mcp]，并且能读取前面模块介绍过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
