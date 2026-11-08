---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流往往包括查找所需文件、修改代码、运行测试、调试、发现遗漏的文件并再次更新……这样的工作还会持续下去。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式能在 IDE 中较自主地工作。它会先探索现有项目结构、执行必要更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但它们针对不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此希望允许用户按类别和发布者筛选。上一练习已添加发布者辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先浏览 Tailspin Toys 网站，了解它目前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个单独的 Astro 应用，页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有独立后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行网站：

1. 返回 codespace。上一练习结束后应已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux)，在 Codespace 中打开新终端。
3. 更新代码并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd> (Mac) 或 <kbd>Ctrl</kbd> (Windows/Linux)，然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，它会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向当前运行 Web 服务器的 codespace 的私有隧道。

### 浏览网站

网站运行后，可以浏览现有功能：

- **主页**：显示桌游列表，包括名称、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页面，显示游戏名称、说明、发布者和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。模式和模型因练习而异；每一步会在需要时说明。

网站的初始实现已经可用，但我们想添加新功能。先查看待办事项。通过模板创建项目时，系统会自动创建一组议题；让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 工作坊作者并非推荐某个模型。构建本工作坊时使用了 Claude Sonnet 4.5，因此将它写入说明，希望代码建议相对一致，以便提供良好体验。不过，LLM 的结果具有概率性，你可能会看到与说明不同的建议。这完全正常。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. 选择 **Continue** 运行列出所有议题的命令。
5. 查看生成的议题列表。

注意 Copilot 还会根据对用户的帮助程度为项目排序。

## 查看说明文件

让智能体生成代码前，先查看要用于提供工作上下文的说明文件。接下来使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 文件，其中包含如何为网站添加功能的上下文。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - Astro、Tailwind CSS 和 Drizzle 等文件类型的说明链接。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用说明、将大型指南拆分成较小文件，并指向示例和模板。

## 实现筛选功能

完成筛选功能至少需要进行三项更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前需要运行并通过测试。现在让 Copilot 智能体模式完成这些任务。

1. 可以继续当前 Copilot 会话，也可以选择 **New Chat** 开启新会话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 说明文件。

   ![选择 UI 说明文件](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用与待办议题相关的以下提示，让 Copilot 实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务复杂度和代码库规模，可能需要几分钟。期间编辑器中可能显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项；工作尚在进行时无需选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格中请求用户确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这没有关系。Copilot 会在生成代码和运行测试之间迭代，直至任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 务必审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认一切正常并完成代码审查后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。本练习中，我们了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

现在通过 [Playwright MCP 服务器测试功能][next-lesson]，然后为功能创建拉取请求。

### 加分探索练习——实现分页

随着游戏列表不断增加，网站需要提供分页。运用本练习学到的技能，让 Copilot 更新网站以实现分页。编写提示时可以考虑：

- 遵循现有最佳实践，包括使用已有说明文件。
- 决定分页的实现方式，例如是否允许用户选择每页大小，还是固定大小。
- 在提示中提供必要的指南，以便 Copilot 按照你的预期实现功能。
- 你可能需要多次迭代，要求 GitHub Copilot 修改实现并补充上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：用途及适用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
