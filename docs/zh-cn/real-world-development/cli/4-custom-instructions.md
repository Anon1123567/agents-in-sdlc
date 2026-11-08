---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。若任务需要按特定方式完成，应确保 Copilot 能获得相关指导。[说明文件][instruction-files]不仅描述想要什么代码，还会说明代码应如何组织。现在已经实现筛选功能，接下来查看 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一套开发规范，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值（如有可注入的 `db` 参数也应说明）。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 了解这些实践要求。

## 说明文件

自定义说明会提供上下文和偏好，帮助 Copilot 了解编码风格和要求。可以指定编码规范、库，甚至喜欢的注释形式，从而得到更相关的建议和代码片段。说明既可以适用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会提供给 Copilot。内容应与大多数请求相关。
- `.github/instructions/*.instructions.md`：用于特定语言、文件类型或任务的指南。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。使用特定格式前，请查看[自定义说明支持参考][custom-instructions-support]。

## 查看此项目中的自定义说明文件

项目中已包含一组说明文件。先查看它们，了解现有内容。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。该文件适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看其中的说明文件。这里有 Astro、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意其 `applyTo` 字段，该字段用于指定说明适用的文件 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他说明文件和项目现有文件的引用。说明文件可以拆分为可复用的小块，并指向适合遵循的示例。

## 更新说明文件以符合团队指南

现有文件已经很有帮助，但还需要一个要求。我们要修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约第 35 行）。
2. 在最后一条后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的说明

Copilot CLI 会在对话启动时加载存储库说明。保存说明后，退出并恢复筛选会话，使 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选会话仍然打开，输入 `/exit` 关闭。Copilot CLI 会自动保存会话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的会话：

   ```bash
   copilot --resume
   ```

   如果出现提示，选择筛选会话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和下一步

你已了解 Copilot 如何使用本项目的说明文件，并应用了新的标准。本课中，你：

- 了解了存储库说明和按路径限定的说明文件如何提供给智能体。
- 更新了说明文件，确保遵循编码标准。
- 观察了说明文件对代码的影响。

接下来[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
