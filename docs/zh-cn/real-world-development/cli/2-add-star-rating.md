---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，就可以开始对项目进行第一次更改了。先从小处着手：游戏数据中已有星级评分，但首页的游戏卡片尚未显示它。你将让智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

在本课程中，你将：

- 在功能分支上启动专注于单项任务的 Copilot 对话；
- 让智能体对项目进行小幅更改；
- 使用 `/diff` 审查更改；
- 运行应用，并在转发的浏览器中确认更改；
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示了评分。不过，首页的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，你将让智能体在每张卡片上显示已有评分。这项更改范围小且独立，非常适合作为第一次会话的任务。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通的 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是专用工作树。保存的对话可让你稍后继续同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话包含三项内容：你的提示和智能体的回复、智能体探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

启动新对话以实现功能：

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果尚未启动 Copilot，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。你已使用 Copilot CLI 添加了新功能！

## 审查差异

即使是小改动，也要在合并前审查所有 AI 生成的更改。下面直接在 Copilot CLI 中查看更改：

1. 输入 `/diff`，检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数值，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的行为具有概率性而非确定性，因此生成的代码可能不同。审查实际行为，不要期待与某个特定实现完全一致。

## 检查更改

不要只阅读代码就假定一切正常。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中查看更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五分制评分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成！接下来创建拉取请求（PR），将代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 在按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux）的同时选择 Copilot CLI 显示的 URL，打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和下一步

恭喜你使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上启动了专注于单项任务的 Copilot 对话；
- 指示智能体对游戏卡片进行小幅更改；
- 使用 `/diff` 审查更改；
- 运行应用，并在转发的浏览器中确认星级评分；
- 创建并合并了第一个拉取请求。

接下来，[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]实现更大的功能。

## 资源

- [GitHub Copilot CLI 简介][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
