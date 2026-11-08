---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，并定义清晰的 QA 流程。

本课将：

- 了解自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其发现。

## 场景

在创建拉取请求 (PR) 前，Tailspin Toys 希望一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的专用 Copilot。配置文件说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

不同的自定义项各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复运行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何利用这些能力评估需求并报告发现。它们不会取代彼此，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。它会定义 QA 的检查项目和必须遵守的边界。

1. 返回 Codespace，确认筛选会话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查其配置，确认 Copilot 捕获了预期的 QA 流程和边界，避免它在只要求验证功能时进行不必要或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但可提供清晰的显示名称。
3. 阅读配置文件说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会先询问，并且不会提交更改或打开拉取请求。
5. 如果生成的配置文件缺少任何职责或边界，请让默认智能体先修改再继续。

## 根据议题运行 QA

创建配置文件后，Copilot CLI 会在会话启动时加载项目智能体。恢复筛选会话，然后选择 QA，使其可以使用会话中的议题和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会立即出现在可用智能体列表中。它仍可通过上述命令启用。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结和下一步

你已添加可复用的专用角色，并审查了它的工作。本课中，你：

- 了解了自定义智能体如何结合说明、技能和 MCP 工具。
- 创建并检查了可复用的 QA 配置文件。
- 选择 QA 智能体，根据议题和规划决策审查功能。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来将它们汇总到功能 PR 中，并使用 Agent Merge。[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
