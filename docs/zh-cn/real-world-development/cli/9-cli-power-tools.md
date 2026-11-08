---
title: "课程 9 - GitHub Copilot CLI 斜杠命令"
description: "探索管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可用于访问高级功能、后台信息和其他配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。接下来看看其他实用命令。

本课将：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索并行工作、工作树以及委派给云智能体等可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他功能，包括管理上下文、切换模型、共享会话，以及可选地委派任务给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以通过斜杠命令检查或手动压缩。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及上下文在系统说明、工具、消息和剩余空间之间的分配情况。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话原本很短，变化可能不明显。

> [!NOTE]
> 上下文窗口接近上限时，Copilot CLI 会自动压缩上下文。若要自行决定时机，可使用 `/compact`。如果切换到无关任务并希望开启新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有各自偏好。Copilot CLI 可以列出并选择模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队合作和分享经验有助于每个人改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布为 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，导出到本地 Markdown 文件。
3. 发送或发布导出内容前先进行检查。会话导出内容可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。不要发布团队不打算共享的存储库或会话内容。

## 可选：扩展工作或委派任务

核心工作坊已完成。Copilot CLI 还为更大规模的任务提供了命令：

- `/fleet` 可将独立子任务分配给子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务交给 Copilot 云智能体异步处理，它可能会创建拉取请求。

这些命令是可选的，因为它们可能创建额外工作树或触发远程工作。请先创建新的、范围明确的任务，并通过正常工作流审查结果。若要进一步了解异步智能体工作，可继续学习[云智能体工作坊][cloud-workshop]。

## 总结和下一步

Copilot CLI 斜杠命令可用于配置 CLI、共享会话并查看后台状态。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出会话。
- 探索并行工作、工作树以及委派给云智能体等可选命令。

Copilot CLI 还有更多命令和功能可供探索。现在通过[回顾所学内容][next-lesson]和后续资源结束本系列。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
