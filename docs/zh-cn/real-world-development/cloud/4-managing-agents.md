---
title: "练习 4 - 监视和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几课中，你让 Copilot 云智能体处理了三项任务，目标是改善用户体验并添加功能。云智能体可以异步、自主地工作，但监视这些任务也很重要。

有多种工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个 mission control 中，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，甚至引导进行中的会话。

本课将：

- 使用智能体页面监视云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以创建高对比度模式后，团队意识到也可以同时添加浅色模式。既然正在更新网站样式并添加切换功能，就适合一并实现。现在要引导智能体，同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 前往 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表应包含已分配给 Copilot 的任务，例如：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤取决于 Copilot 的工作状态和所采用的方法。

4. 也请注意右侧显示的拉取请求窗格。可以通过它查看 PR 和已更改的文件，以便进一步监视。

## 引导云智能体

既然已经查看正在进行的任务，现在让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式有关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示的引导活动云智能体任务对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务。完成发送消息时正在执行的具体步骤后，它会将新请求加入工作流程。

这需要几分钟，可以借此暂停并回顾到目前为止学到和尝试的内容。

## 总结和下一步

本课介绍了 Copilot 智能体页面——监视和引导 GitHub Copilot 云智能体任务的中心位置。借助这个 mission control，可以跟踪所有进行中和已完成的任务、审查正在执行的工作，甚至重定向进行中的任务来调整范围或补充说明。

本课介绍了以下概念：

- 使用 mission control 和智能体页面监视云智能体任务。
- 重定向正在进行的会话，以请求添加其他功能。

Copilot 正在完成无障碍功能，接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
