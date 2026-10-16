---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按特定方式完成，或者 Copilot 需要了解背景信息，请确保它能获取相关上下文。可以通过多种方式向 Copilot 提供特定上下文，其中最重要的方式之一是使用[说明文件][instruction-files]，为代码生成提供指导。

本练习将：

- 探索项目上下文、编码指南和文档标准如何通过存储库自定义说明和按路径限定范围的说明传递给 Copilot。
- 在现有说明的基础上生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行同一提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交并合并到 `main`，让 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不会完全遵循你设置的标准。Copilot 的输出并非确定性的。本练习关注更新说明后行为变化的*趋势*，而不是要求生成结果与示例逐字一致。

## 说明文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求：

- 数据层始终需要单元测试。
- UI 应采用暗色模式，且具有现代感。
- 应通过 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 获得正确的信息，并根据这些实践完成任务。

### 自定义说明

自定义说明可提供上下文和偏好，帮助 Copilot 更好地了解你的代码风格和要求。可以指定首选编码约定、库，以及代码注释的编写偏好等。可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

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

- 将 `copilot-instructions.md` 中的内容限定为项目级指导，例如项目简介、项目结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供说明。
- 使用自然语言，清晰地描述指南，并提供代码应当如何编写（以及不应如何编写）的示例。

说明文件没有唯一正确的编写方式，使用 AI 也是如此。通过试验找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应当有一套完善的说明文件。浏览本项目的文件时，可以看到其中涵盖多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面提供的方式各不相同（例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）；当前环境路径的课程会在相关步骤中介绍。
>
> 想找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含各种说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义说明文件

花些时间阅读存储库中的说明文件：其中有一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 下，还可以看到适用于 **GitHub Actions Workflows** 的嵌套指南。这些内容适用于你使用 Copilot 执行的各种任务。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。这里包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来确定说明适用的文件。此处，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会符合条件。
5. 查看针对本项目单元测试的具体说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。注意其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可将大型说明集拆分为较小、可复用的文件，并提供可供 Copilot 参考的示例。（这些路径相对于说明文件本身，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分说明了项目编码标准，但目前还没有要求在代码中添加文档。接下来，你会添加 TSDoc 文档注释和文件注释标题的规则。

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先在 Copilot Chat 中尝试

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动干净的对话。不同练习使用的模式和模型可能不同，相关步骤会注明需要注意的选项。

为了了解自定义说明的影响，先在使用当前说明文件的情况下发送提示。稍后更新说明，再发送相同提示以比较结果。

1. 关闭之前练习留下的编辑器选项卡，以便 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时更改辅助函数文件和测试。
6. 注意，建议的辅助函数是类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组——这是 `.github/instructions/drizzle.instructions.md`（适用于 `src/lib/*.ts`）中的数据层约定。
7. 留意建议代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的输出具有概率性，因此即使没有明确要求，它也可能会添加文档注释。如果发生这种情况也没关系；本练习的重点是更新说明后行为的一致性有所改善。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该在第 27 行附近。查看其中记录的编码标准；目前还没有关于代码文档的规则，因此刚才生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，可以在存储库级文件（`.github/copilot-instructions.md`）中添加全局指导，也可以通过 `*.instructions.md` 文件为特定语言、文件类型或任务提供说明。你刚添加的文档注释规则属于项目级标准，放在存储库级文件中最合适。

## 重新运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清除对话内容。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送**与之前相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似下面的注释开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意建议的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循一项新的项目标准，并将其应用到真实代码中，下一练习会在此基础上继续。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，也由源代码管理系统进行版本控制。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改均已生成：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有文件更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他配套更改（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，应创建拉取请求，并将其关联到待办列表中的文档标准议题。可以手动执行，不过也可以让 Copilot 使用已连接到项目的 GitHub 工具代为操作。现在让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是第一次在练习中让 Copilot 代你操作 GitHub。项目模板已将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍此连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中了 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，同时描述说明更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 询问是否允许代你执行 GitHub 操作时，审查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 合并 PR 并将分支切换回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代你执行 GitHub 操作和 Shell 命令时，先审查命令，再酌情选择 **Allow**。

现在，你已经在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结与后续步骤

你已探索 Copilot 如何从本项目的说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat 来：

- 发送代码生成提示，观察 Copilot 在使用*现有*说明时生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 重新运行相同提示，观察建议的代码如何采用新标准。
- 将说明更新和辅助函数基础提交并合并到 `main`，让下一练习可以在此基础上继续。

接下来，在 [智能体模式][next-lesson]中应用这些说明，让 Copilot 跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot — 说明文件和其他资源集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
