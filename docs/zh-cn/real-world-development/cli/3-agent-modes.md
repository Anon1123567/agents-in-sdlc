---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据议题构建筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作方式，帮助我们以正确方式构建正确功能。本系列课程将体验典型的智能体开发流程：从议题着手生成新功能，确认代码有效、功能符合预期，最终成功合并到项目。

> [!NOTE]
> 后续功能工作将沿用同一会话和分支。通常会为不同类型的文件创建不同分支或 PR，但本实验会采用捷径，让你专注于核心概念。

本课将：

- 从 GitHub 议题启动 Copilot 新会话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发布者筛选游戏。待办议题描述了该功能，但组合类别等细节需要先达成一致。你将使用 Plan 模式确定这些决策，然后授权 Autopilot 执行范围明确的实现。

## 背景

在开发流程中引入 AI 编码智能体不会改变基本原则，反而使这些原则更加重要。大多数开发者的流程大致如下：

1. 从描述需求的议题开始。
2. 规划要构建的内容。
3. 构建并审查代码。
4. 运行测试来验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多与上述流程相似。

遵循标准流程可以确保 AI 生成的代码符合要求，并像手写代码一样经过审查。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可循环切换模式：

- **Interactive**：你和智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式，审查计划后再用 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
4. 更新 `main`，并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题加入提示并返回 **Session** 选项卡。

注意提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 加入上下文。

## 规划筛选功能

规划可以先确定功能实现或任务处理方式，再交给 Copilot。复杂任务尤其适合先花时间规划。现在切换到计划模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方式。

3. Copilot 可能会询问筛选功能的细节。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) 展开计划。
5. 浏览并审查计划。
6. 要求 Copilot 修改任何不符合你决定的部分。

## 批准 Autopilot

计划撰写并审查完毕后，就可以开始实现。使用 Autopilot 让 Copilot 执行任务。

Autopilot 会让 Copilot 持续迭代，直到它认为任务完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中类似的选项。
2. 确认提示下方显示 **Autopilot** 模式。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决环境问题，再将检查视为已完成。

## 审查并验证实现

生成代码后，和其他代码一样，必须先审查再合并。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据议题和规划时作出的决定审查结果。
4. 审查完代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果；如有失败，请让 Copilot 解决。

## 体验新功能

代码看起来没问题，但是否真的能运行？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发布者筛选，以及计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动了 Copilot 新会话。
- 在 Plan 模式中明确了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来通过[使用自定义说明][next-lesson]深入了解如何生成符合规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
