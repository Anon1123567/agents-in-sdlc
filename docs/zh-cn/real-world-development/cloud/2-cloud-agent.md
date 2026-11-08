---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建一个草稿拉取请求，供开发者审查。这样可以把繁琐任务交给智能体，加快开发进度，让开发者专注于更重要的工作。

本课将使用 Copilot 云智能体了解如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初创建网站的承包商没有留下完善的文档，事实上，代码完全缺少文档注释。团队首先希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。暂时不需要完善的实现，只需要一个可用于验收测试用户体验的版本。目前这项工作受阻，但其他议题的优先级更高。

这两项任务都可能很快被降级为低优先级，适合分配给 Copilot 云智能体。它可以异步处理任务，让开发者先完成其他工作，之后再回来审查 Copilot 的成果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。与人工开发者协作类似，你可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。成果准备好供审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明

GitHub Copilot 并非魔法工具，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能包含复杂细节，因此要注意[如何为 Copilot 云智能体界定任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、尝试并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果你已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub 网页界面中打开以下文件；如果已启动 codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意其 `applyTo` frontmatter，它将说明限定为 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它生成的 TSDoc 注释、TypeScript 规范和文件注释头；这些都由说明文件引导。稍后的练习还会再次审查拉取请求。

## 为 GitHub Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要配置特定环境并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 时也是如此，因为它会像软件工程师一样处理任务。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。你可以创建一个[专用设置工作流][setup-workflow]，在它开始工作前自定义环境。该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 中，可安装所需的开发工具和依赖项。实验所需配置已预先准备好，可确保 Copilot 获得 Node.js、项目依赖项、端到端测试所需的浏览器，以及用于 Astro 应用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你在存储库的 "Actions" 选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为 *你的步骤* 设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流相同，但有几个关键点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，该作业会在 GitHub Actions 中运行。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以先验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但许多项目的文档已经过时，或者根本没有文档。这类技术债务经常被搁置，会拖慢工作效率，也让维护代码库或引入新开发者更加困难。Copilot 擅长生成文档，因此很适合将这项任务分配给云智能体。它会在后台生成所需文档，后续练习再审查结果。

1. 在新的浏览器选项卡中，前往 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表及其计划完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并决定计划，以及创建代码。

这可能需要几分钟。云智能体的主要优势之一是异步执行任务，让我们能专注于其他工作。接下来会再分配一项任务，同时编写代码为应用添加功能，充分利用这一优势。

## 在游戏详情页添加相关游戏版块

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。即使为设计团队添加相关游戏版块不会花很长时间，也可以把这段时间用于其他工作。现在就把任务分配给云智能体。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

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

不久后，议题的第一条评论中会出现 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体会像软件工程师一样工作；无需持续监控，完成后再审查即可。现在可以转向创建和使用自定义智能体。

## 总结和下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建和使用自定义智能体。云智能体还可以使用 [MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳做法][cloud-agent-best-practices]

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
