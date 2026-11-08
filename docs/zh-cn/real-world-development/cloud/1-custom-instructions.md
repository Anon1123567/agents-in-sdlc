---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定要求，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅理解要完成什么，也理解应该如何完成。

在本练习中，你将：

- 了解项目专属上下文、编码指南和文档规范如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级规范。

> [!NOTE]
> 与 VS Code 和 CLI 练习不同，本练习不会比较提示的前后效果。Copilot 云智能体会异步处理 GitHub 议题，很难实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，你会看到指令文件如何影响其结果。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层必须始终包含单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些实践要求并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。你可以指定编码规范、库，以及希望添加的注释类型。可以创建适用于整个存储库的指令，也可以创建适用于特定文件或任务的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会将这个单独的指令文件发送给 Copilot。文件应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这类文件，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他规范的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整介绍如何创建指令文件超出了本练习的范围。示例项目展示了一种有代表性的方法，主要原则如下：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目说明、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应该和不应该采用的写法。

创建指令文件或使用 AI 没有唯一正确的方法。请根据项目和场景不断试验，找到适合自己的做法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应准备一组完善的指令文件。浏览此项目的文件时，你可能会看到针对多种任务的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本练习会在相关步骤中介绍对应操作。
>
> 如需模板或起点，请浏览 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目的自定义指令文件

花些时间查看此存储库提供的指令文件：一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。特别注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。注意这里有 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意文件顶部的 `applyTo` 字段；它使用相对于存储库根目录的 glob，指定适用文件。此处匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看此项目有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意它对其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可重复使用的文件，并为 Copilot 提供代码示例。（其中的路径相对于该指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但目前还没有要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级规范

接下来要编辑一次文件：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释头记录代码文档。之后分配议题时，云智能体会采用这条规则。

编辑前，先创建并切换到一个新分支（云智能体会从议题目标分支读取指令，但仍应以正常方式提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码规范有助于改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。查看其中记录的项目编码规范；目前尚无代码文档规则，因此生成的辅助函数没有文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以使用 `*.instructions.md` 文件，为特定语言、文件类型或任务提供指导。刚刚添加的文档注释规则属于项目级规范，因此适合放在存储库级指令文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将把议题分配给 Copilot，而它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。之后，云智能体就能在下一练习中使用这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处使用分支是为了模拟真实项目中的工作方式。

## 总结和下一步

你了解了 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了新的存储库级规范。接下来几节中，云智能体创建的拉取请求会体现这条规范。

下一步，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
