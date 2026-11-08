---
title: "课程 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续工作流。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案、在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并用它在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 交付成果

本工作坊分为两个 PR 阶段，每个阶段都基于更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流：**实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置文件并添加相关测试。

从规划筛选功能到创建 PR，你一直使用同一会话和分支。为简化工作坊，这些更改合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自使用浏览器检查，以及通过 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查结合起来，为 PR 提供完整依据。

新增测试应解决实际缺口；如果 QA 检查后认为无需新增测试，也可能是正确的。授权合并前应审查代码和证据，并在更改后重新生成受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义技能、创建 QA 配置文件并配置 MCP 服务器。可以在多个会话中复用这些自定义设置，并随团队需求调整。说明定义标准，技能描述可重复的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。应审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先确定方案，使用 **Interactive** 关注小范围更改，使用 **Autopilot** 处理范围明确的任务。例行编辑可选择更快的模型；复杂工作则选择能力更强的模型。

上下文仍与基础设施同样重要。明确描述希望构建的内容、原因和方式，会显著影响输出。

## 继续探索

核心工作流已完成。还可以了解一些 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是持续实践。可以将它用于生产代码、个人项目，或多年一直想做的小应用。与团队分享经验，也向其他成员学习。别忘了继续查阅文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较不同环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
