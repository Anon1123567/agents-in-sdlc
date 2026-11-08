---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理为可复用的智能体，例如执行 [WCAG][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents` 中。每个文件都包含至少有 `name` 和 `description` 字段的 YAML frontmatter，后接用于定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并负责协调技能（甚至其他智能体）完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能，或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的正确方式。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的限制。近期用户反馈指出，当前深色主题的文字与背景对比度不足，难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开关的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来会使用自定义智能体生成该功能。

本练习将：

- 查看现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

已经为无障碍任务创建了一个自定义智能体。先查看它的内容，了解它将如何引导 Copilot。

返回 codespace，然后打开终端，从 `main` 创建新分支，用于单独处理无障碍功能（筛选 PR 将保留为练习 3 中独立的 PR）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 浏览后续各节，了解其中列出的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。可以选择智能体，让对话遵循该智能体的指令和工具设置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。模式和模型因练习而异；每一步会在需要时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（默认显示当前活动智能体 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并修改项目文件。每项修改都会在 Chat 中内联显示文件路径和差异，供你审查。
6. 随着修改应用，**Files changed** 指示器会更新，显示智能体已修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库文件，包括 Astro 组件、CSS 和相关测试。

下一课将审查并引导这项正在进行的工作。

## 总结和下一步

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的智能助手。利用自定义智能体，可以将团队专业知识和标准整理为可复用的智能体，引导 Copilot 更有效地完成工作。

本课介绍了以下概念：

- 如何定义自定义智能体。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来[监视并引导智能体工作][next-lesson]——审查进行中的更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 上的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监视和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
