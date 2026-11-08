---
title: "练习 5：迭代改进 GitHub Copilot 的成果"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 合作完成了多项任务，以改善用户体验并添加功能：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式等无障碍功能。接下来查看代码更改，必要时向 Copilot 提供反馈，改进其成果，然后再为团队其他成员启用审查。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。现在先审查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## 安全和 GitHub Copilot 云智能体

Copilot 云智能体异步且无需监督地执行任务，因此设置了一些安全限制：

- Copilot 只能读取存储库，并且只能写入它用于提交代码的分支。
- 云智能体在 GitHub Actions 中运行，并在独立的临时环境中工作。
- GitHub Actions 工作流必须经过人工批准才能运行。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。通过 GitHub.com 的标准 PR 界面完成此操作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问许多外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 或更完整描述的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。可以利用这段时间休息，或回顾已学内容。

4. PR 准备好后，选择 **Files changed** 选项卡并查看更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新代码，其中包括 TSDoc 文档注释和其他文档。具体更改可能有所不同。检查其中是否包含 TSDoc 文档注释、TypeScript 规范和注释头；这些都由练习 2 中查看的自定义说明文件提供。
6. 审查完毕且确认一切正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应出现在拉取请求的检查部分。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作并非单向过程。你可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会看到这些评论，并启动新会话来处理请求。由于结果具有非确定性，无法规定必须使用的具体文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论请求 Copilot 修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的建议，或提出其他代码库文档改进请求。
2. 选择 **View Session**，观察 Copilot 执行修改。注意 Copilot 会启动新会话。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成修改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。完成后，可以将 PR 从草稿转换为准备审查状态，然后合并到 main 分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决游戏详情页显示相关游戏这一议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改正常后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. 工作流应出现在拉取请求的检查部分。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已创建相关游戏功能。与之前一样，可以通过评论持续迭代，要求云智能体调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，这项任务是在更新自定义说明**之前**分配的）。在 **Conversation** 选项卡中添加评论即可请求更改，Copilot 会启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。这个 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点检查：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保留用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更改正常后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. 工作流应出现在拉取请求的检查部分。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后设置仍然有效。

注意自定义无障碍智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍问题或可改进之处，可以像之前一样在评论中标记 **@copilot** 请求更新。

## 可选练习 — 继续委派任务

云智能体适合处理待办事项，让你专注于其他工作。可以在自己的存储库中再创建几个议题并分配给 Copilot，以养成这种习惯。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体流程。在这些课程中，你：

- **查看了存储库中的自定义说明文件**，以便观察它们对云智能体输出的影响。
- **向 Copilot 云智能体分配议题**，并观察它设置环境、制定计划和异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，包括高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为 mission control**，监视任务并在会话进行时引导无障碍工作。
- **审查并迭代云智能体的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和下一步

云智能体流程已完成。如果想继续探索，其他流程可以补充本次练习：

- 🖥️ **[VS Code 流程](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 流程](../../cli/)** — 在终端中交付两项经过审查的更改，并使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳做法][cloud-agent-best-practices]
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
