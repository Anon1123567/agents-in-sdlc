---
title: "练习 1 - 自定义说明 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]来指导代码生成。

本练习将：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发布者辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和筛选代码切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不符合所有设定的标准，因为 Copilot 的结果具有非确定性。本练习旨在观察更新说明后行为的*趋势*，而不是要求输出与示例完全相同。

## 说明文件

### 场景

Tailspin Toys 制定了一套开发指南，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加描述文件用途的注释块。

通过说明文件确保 Copilot 知道这些实践要求。

### 自定义说明

自定义说明会提供上下文和偏好，帮助 Copilot 了解编码风格和要求。可以指定编码规范、库，甚至喜欢的注释形式，从而得到更相关的建议和代码片段。说明既可以适用于整个存储库，也可以针对特定文件类型或任务。

说明文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会提供给 Copilot。文件应包含与大多数请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可用于特定任务或文件类型，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的说明文件，包括 AGENTS.md 和 CLAUDE.md，详情请参阅[自定义说明支持参考][custom-instructions-support]。

### 管理说明文件的最佳实践

关于创建说明文件的详细讨论超出本课程范围，但示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 聚焦于项目级指南，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供专门说明。
- 使用自然语言，保持说明清楚，并提供正反例。

说明文件没有唯一的正确写法，AI 的使用方式也一样。通过试验找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备完善的说明文件。查看本项目的说明文件时，会看到 UI 更新和 [Astro][astro-instructions] 等任务也有对应的说明。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应界面的课程会在相关处介绍。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间阅读此存储库包含的说明文件：一个核心 `copilot-instructions.md` 文件，以及多个用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。它们适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，浏览其中的文件。这里有 Astro、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定相对于存储库根目录的 glob。任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些说明。
5. 查看此项目的单元测试专用说明。
6. 最后打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它引用其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件。通过这种方式，可以拆分并复用说明，还能提供示例（路径相对于说明文件所在位置）。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍项目编码标准，但尚未要求添加代码文档。接下来会添加 TSDoc 文档注释和文件注释头规范。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) 打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新会话。模式和模型因练习而异；每一步会在需要时说明。

先使用现有说明发送提示，再更新说明并重新发送相同提示，以便观察差异。

1. 关闭之前练习留下的编辑器选项卡，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并修改代码，通常会同时更新辅助函数文件和测试。
6. 注意生成的辅助函数是带类型的函数，接收 `db` 客户端作为第一个参数，并返回带类型的发布者数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层规范。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它有时可能会在没有明确要求时添加文档注释。这也没问题；本练习重点是观察更新说明后的一致性变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 为 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约第 27 行）。此部分描述编码标准，但还没有代码文档规则，因此刚生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以将说明放在存储库级别的 `.github/copilot-instructions.md` 中，作为全局指南；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。像文档注释这样的全局标准适合放在存储库级文件中。

## 重新发送提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除缓冲区。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用于下一练习要继续使用的真实代码。

## 提交并推送分支

说明文件与其他项目文件一样，需要通过源代码管理来维护。现在将其提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) 打开新终端。
2. 在终端中检查说明更新和辅助函数更改是否存在：

   ```bash
   git status
   ```

3. 暂存说明更新和 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

推送分支后，创建拉取请求并关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用项目已连接的 GitHub 工具。让 Copilot 查找议题，创建包含关闭议题说明的 PR，然后合并。

> [!NOTE]
> 本练习会首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示操作即可。后续练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) 打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新说明文件有关的议题，并从当前分支创建 PR，说明其中包括说明更新和新的发布者辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 如果系统询问是否允许 Copilot 代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，使用以下提示让 Copilot 合并 PR 并让本地分支返回 main：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统询问是否允许 Copilot 执行 GitHub 操作和 shell 命令，请审查命令并酌情选择 **Allow**。

现在已借助 GitHub Copilot 创建并合并拉取请求。

## 总结和下一步

你已了解 Copilot 如何使用本项目的说明文件，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在*现有*说明下生成的内容。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送相同提示，观察生成的代码如何遵循新标准。
- 将说明更新和辅助函数基础提交到 `main`，为下一练习做准备。

接下来在[智能体模式][next-lesson]中运用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个提示][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
