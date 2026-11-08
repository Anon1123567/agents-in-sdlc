---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "查看或配置 Copilot CLI 中的 Playwright MCP，并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还要处理数据、外部服务，并允许 Copilot 使用其他自动化工具。MCP 服务器可以扩展 Copilot CLI 内置的能力，提供更多工具和服务。

本课将：

- 了解什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未配置时添加它。
- 让智能体操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与界面交互。我们要允许 Copilot 像用户一样操作网站，以便自动化检查更改是否符合预期，从而提升信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

AI 智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁。每个 MCP 服务器都会提供一组工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并参与生态建设。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，以便 Copilot 像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) 保存 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前已手动确认功能符合预期。现在使用 Copilot CLI 中的 Playwright MCP 服务器执行同样的检查。

1. 使用以下提示，让 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确指定 MCP 服务器，Copilot 会自行判断。不过，明确指出工具有助于保持一致，并节省一些 token。

2. 观察 Copilot 在浏览器中执行检查步骤。
3. 阅读报告，确认结果符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后向你提供报告。

## 总结和下一步

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中体验功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在 Playwright MCP 尚未配置时添加了服务器。
- 让智能体操作浏览器并体验筛选功能。

接下来创建一个[质量保证 (QA) 自定义智能体][next-lesson]，将技能与浏览器工具结合到专门角色中。

## 资源

- [MCP 是什么，为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
