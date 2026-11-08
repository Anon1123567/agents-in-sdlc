---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式实现了筛选功能。创建拉取请求前，应先在浏览器中确认功能确实可用。接下来连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，而不是由你手动逐项检查；然后发布分支并创建 PR。

本练习将：

- 了解什么是 Model Context Protocol (MCP)，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选更改创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以访问编辑器之外的资源，从而发挥更大作用。Copilot 使用 Model Context Protocol (MCP) 实现这种能力——它是一种智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，显示它如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器负责管理智能体与外部工具的通信，例如现有 API 或 npm 包等本地工具。每个 MCP 服务器都会提供一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用它创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可提供不同的工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态建设。

> [!CAUTION]
> 从安全角度看，应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` 条目包含在项目模板中，因此上一练习中 Copilot 可以读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，并在 Copilot 首次使用工具时让你使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在本机运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在后台运行浏览器，不显示窗口。在没有桌面环境的 codespace 中，必须使用此设置。

> [!NOTE]
> Tailspin Toys 项目已在端到端测试中使用 Playwright，因此通常已经安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要工具时启动服务器，并询问是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操作真实浏览器。无需亲自选择界面，智能体可以打开页面、导航、应用筛选并读取结果，然后总结观察结果。这是无需离开对话即可快速确认功能是否符合预期的方式。

Playwright MCP 服务器使用页面的[无障碍树][playwright-mcp-server]而不是屏幕截图。智能体会根据结构化且带标签的元素（按钮、链接、列表项）进行推理，与辅助技术的工作方式相同。因此，快速功能检查也能顺便验证一些基本无障碍行为。

服务器已连接且应用正在运行后，请让 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行每个步骤，然后报告结果。根据议题中的验收条件审查报告；如果发现异常，请进一步询问或让 Copilot 修复代码，然后再创建 PR。

> [!NOTE]
> 测试时应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。先发布 `filtering-vscode` 分支。

1. 前往 codespace 中的 **Source Control** 面板，审查 Copilot 所做更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示的更改](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 GitHub.com 或 GitHub 命令行界面 (CLI) 等方式创建拉取请求。也可以让 Copilot 使用已连接的 GitHub 工具查找议题并创建关联的 PR。

1. 前往 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确认在智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会要求验证 GitHub 身份；按照提示操作即可。
5. 查看 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 提供的链接以查看 PR，但暂时**不要**合并。

## 总结和下一步

恭喜！本练习中你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它让 Copilot 在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来创建[自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
