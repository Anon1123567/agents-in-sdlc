---
title: "课程 8 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能、说明更新、技能更新、质量保证 (QA) 配置和测试都保存在同一分支。现在一并审查这些内容并创建拉取请求。之前你手动合并了星级评分 PR，这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会把功能、说明更新、技能更新和 QA 智能体拆分到几个独立的 PR。为简化本工作坊，我们在同一会话和分支中完成筛选及质量工作，并将所有更改放在同一个 PR 中。

本课将：

- 了解 Agent Merge 以及它如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余 PR 工作，同时让开发者保留合并授权权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 当前的阻碍，例如修复持续集成 (CI) 检查失败、回应审查评论，以及在需要时变基；之后在存储库允许的情况下启用 GitHub 自动合并。

此前一直由你选择 **Merge pull request**。Agent Merge 可以代为处理这项工作。启用前，先审查工作并确认可以合并。

## 使用 Agent Merge 管理 PR

代码完成后，一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 将监视 PR 的持续集成过程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限不足问题。如果合并受阻，请阅读报告的原因，并在存储库允许时按正常审查流程手动完成合并。

## 总结和下一步

你已自动化开发流程中的多个部分，包括生成代码、测试和验证代码，以及管理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

接下来[探索实用的 Copilot CLI 斜杠命令][next-lesson]，管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
