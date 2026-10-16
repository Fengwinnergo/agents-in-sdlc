---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解背景信息，请确保它能访问这些上下文。[说明文件][instruction-files]可提供相关指导，让 Copilot 不仅了解你要完成*什么*，也了解应当*如何*完成。

本练习将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定范围的说明传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境路径不同，本练习不会使用前后对比提示，因为 Copilot cloud agent 会在 GitHub 议题中异步工作，难以实时并排展示效果。稍后审查 cloud agent 创建的拉取请求时，就能看到说明文件的影响。

## 说明文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，且具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可确保 Copilot 获得正确的信息，并根据这些实践完成任务。

### 自定义说明

自定义说明可提供上下文和偏好，帮助 Copilot 更好地了解你的代码风格和要求。可以指定编码约定、库，以及注释偏好等。可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送的单个说明文件。它应包含项目级信息，即大多数 Copilot Chat 或 CLI 请求都相关的背景信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建的说明文件，例如 TypeScript 或 Astro 文件，也可以用于创建 UI 组件或一组新的单元测试。

> [!NOTE]
> 在 IDE 中工作时，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整介绍如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种典型做法，大致包括：

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如构建内容的简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，清晰地描述指导，并提供代码应当如何编写（以及不应如何编写）的示例。

说明文件没有唯一正确的编写方式，使用 AI 也是如此。通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应当有一套完善的说明文件。浏览本项目的文件时，可以看到其中涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；对应环境的课程会在相关步骤中介绍。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含各种说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

花些时间阅读存储库中的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简要介绍以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还可以看到适用于 **GitHub Actions Workflows** 的嵌套指南。这些内容适用于你使用 Copilot 执行的各种任务。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。这里包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来确定说明适用的文件。此处，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 查看针对本项目单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可将大型说明集拆分为较小、可复用的文件，并提供可供 Copilot 参考的示例。（这些路径相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但目前还没有要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释标题的规则。

## 添加新的存储库标准

接下来要编辑一处内容：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释标题。稍后本环境路径中分配的议题会让 cloud agent 使用这项规则。

编辑前先创建一个分支（cloud agent 会读取议题目标分支上的说明，但仍应通过提交整洁地记录更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。查看其中记录的项目编码标准；目前还没有关于代码文档的规则，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级文件（`.github/copilot-instructions.md`）中添加全局指导，也可以通过 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。你刚添加的文档注释规则属于项目级标准，放在存储库级文件中最合适。

## 提交、推送并合并说明更新

Cloud agent 会读取议题目标分支上的说明。下一练习中分配议题时，Copilot 将从 `main` 创建分支，因此必须先将说明更改合并到 `main`，cloud agent 才能使用这些规则。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建一个从 `custom-instructions-cloud` 指向 `main` 的拉取请求并合并。这样，cloud agent 在下一练习处理分配的议题时就会读取这些说明。

> [!TIP]
> 如果希望本工作坊直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。这里采用分支流程，是为了模拟真实项目的工作方式。

## 总结与后续步骤

你已探索 Copilot 如何从本项目的说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。在后续环境路径中审查 cloud agent 创建的拉取请求时，可以看到该标准的应用效果。

接下来，[将第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
