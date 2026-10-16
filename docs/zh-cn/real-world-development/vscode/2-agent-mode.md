---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件并再次修改……类似流程还会继续下去。

这正是 Copilot 智能体模式发挥作用的地方。

Copilot 智能体模式可在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，然后执行必要更新、运行测试等任务，并自动修复发现的问题。下面来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但它们适用于两种不同的体验。智能体模式在 IDE 中执行任务，可快速获得反馈并随时交互。Cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调地进行更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 如何在将更改合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望允许用户按类别和发行商筛选游戏。上一练习中已经添加了发行商辅助函数，现在使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改之前，先探索 Tailspin Toys 网站，了解其当前功能。

此网站是一个以开发者为主题的棋盘游戏众筹平台，可列出游戏并显示详细信息。它是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，可启动单个 Astro 开发服务器，方便运行网站。可以按以下步骤在 GitHub Codespace 中启动：

1. 返回 Codespace。上一练习结束后，应已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似下面的 URL 提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果系统弹出对话框，询问是否要打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），并在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端中选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通向 Codespace 的私有隧道，现在网站服务器就运行在其中！

### 浏览网站

网站启动后，可以体验其功能。网站的主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会进入详情页，查看标题、描述、发行商和类别等信息。

## 与 Copilot 一起探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动干净的对话。不同练习使用的模式和模型可能不同，相关步骤会注明需要注意的选项。

网站的初始实现已具备基本功能，但我们还希望增加新能力。先查看待办事项列表。使用模板设置项目时，系统会自动创建一组议题；请 GitHub Copilot 帮你显示这些条目。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此说明中包含该模型。希望收到的代码建议足够一致，以提供良好体验。不过，由于 LLM 的输出具有概率性，建议可能与本工作坊示例不同，这是正常且符合预期的。

> [!NOTE]
> 由于 LLM 输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 发送以下提示，让 Copilot 查看议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还会根据它认为对用户最有用的程度，为议题排序。

## 审查说明文件

让智能体开始生成代码前，先查看将用于向 Copilot 提供上下文的说明文件。项目提供了[用户界面 (UI) 说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中包含为网站添加功能时的处理方式。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中关于如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置。这有助于复用内容、将复杂说明拆分成更小且易于管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要进行三项独立更新：

- 添加或完善数据层中的筛选逻辑（`src/lib/`）。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并到代码库前必须运行测试并确保通过。Copilot 智能体模式可以帮你完成这些任务！现在添加此功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 启动新对话。
2. 依次选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单仍选中 **Agent**。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认模型仍为 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办列表中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并找到相关文件。它应该会找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成工作后，所有更改都会出现 **Keep** 或 **Undo** 选项，因此无需在它工作时立即选择。

7. Copilot 提示后，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至会看到一些测试失败。这没关系！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且不再检测到错误。

   ![Copilot 智能体模式完整聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 查看生成的代码，检查是否存在潜在问题。

> [!CAUTION]
> 请始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回网站正在运行的浏览器，体验新功能。
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新能力。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调地进行更改。
- 如何在将更改合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

现在使用 [Playwright MCP 服务器测试功能][next-lesson]，并创建拉取请求。

### 额外探索练习 — 实现分页

随着游戏列表不断扩大，需要添加分页。使用本练习所学知识，请 Copilot 更新网站以实现分页。实现时可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 考虑分页的方式：允许用户选择每页项目数，还是使用固定页大小。
- 编写提示时提供 Copilot 所需的指导，以便按预期实现功能。
- 可能需要多次迭代，提供上下文并让 GitHub Copilot 修改代码。这是正常的 Copilot 工作方式！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
