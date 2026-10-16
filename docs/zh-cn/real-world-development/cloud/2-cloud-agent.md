---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务问题，例如未解决的安全问题、需要更新的旧代码，或因缺乏时间而一直积压的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码、添加功能等任务。完成工作后，它会创建草稿 PR，供开发人员审查。这样既能把繁琐任务交出去、加快开发进程，也能让开发人员专注于更重要的工作。

本练习将使用 Copilot cloud agent：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初负责创建网站的承包商没有留下合适的文档，事实上，代码完全缺少文档。团队希望先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队已准备好改善游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，即同类别的其他游戏，以便用户继续浏览。暂时不需要精致的实现；团队只需要可以用于用户体验验收测试的内容。虽然此项工作目前受阻，但还有优先级更高的议题。

这类任务很容易被搁置，非常适合分配给 Copilot cloud agent。Cloud agent 可以异步处理这些任务，让开发人员继续专注于其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发人员一样在后台执行任务。与人类开发人员协作类似，你可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话运行时或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中标记你。

## 清晰界定说明范围的重要性

GitHub Copilot 没有魔法——使用它并不意味着可以跳过思考。看似简单的操作也可能包含复杂细节，因此[请谨慎界定 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程助手：分阶段工作、学习、试验，并根据情况调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

之前的练习介绍了自定义说明及其引导 Copilot 的方式。如果你已经完成了自定义说明练习，就见识过 `.github/copilot-instructions.md` 的作用；如果还没有，现在正好快速查看一下。将工作分配给 Copilot cloud agent 前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在存储库的 GitHub 网页界面或正在运行的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md` — 查看 **Code standards** 部分，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` — 注意 `applyTo` frontmatter，它将说明限定在 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给 cloud agent 时，请留意生成的 PR 中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些内容来自上述说明文件。稍后的练习会再次讨论这一点。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，确保一切准备就绪。将任务分配给类似软件工程师 (SWE) 的 Copilot 时也是如此。

Cloud agent 在 GitHub Actions 中使用自己的环境。可以通过创建[专用设置工作流][setup-workflow]来自定义该环境。该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在 cloud agent 开始工作前运行，以便安装所需开发工具和依赖项。实验室已预先配置此工作流，确保练习顺利进行，并提供相关学习机会。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试用浏览器，以及单个 Astro 应用所需的 SQLite 数据库（包括迁移和种子数据）：

```yaml
name: "Copilot Setup Steps"

# 允许从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅为*自己的步骤*授予所需的最低权限。Copilot 会获得单独的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要授予 `contents: read` 权限。如果设置步骤不克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件类似，但有几点需要注意：

- 工作流只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会先执行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但大多数项目的信息要么已经过时，要么完全缺失。这类技术债务经常得不到处理，会拖慢工作效率，也让维护代码库或引入新开发人员变得困难。幸运的是，Copilot 擅长创建文档，因此这是分配给 Copilot cloud agent 的绝佳任务。它会在后台生成所需文档。在后续练习中，你会回来审查其工作。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新建的拉取请求 (PR)，标题应类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请等待片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 查看待办列表及即将执行的任务。
14. 沿 PR 时间线向下滚动，查看 Copilot 已开始处理议题的提示。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话内容，了解 Copilot 如何处理问题，包括探索代码、理解当前状态、暂停思考并决定合适的计划，以及创建代码。

这可能需要几分钟。Copilot cloud agent 的主要优势之一是可以异步执行任务，让我们腾出时间处理其他工作。接下来利用这一点：再给 Copilot cloud agent 分配一个任务，然后转而为应用添加功能。

## 为游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你专注于一组任务，它同时处理另一组任务。为设计团队添加相关游戏部分也许不会花太长时间，但仍然可以利用这段时间完成其他任务。现在将该任务分配给 Copilot cloud agent！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

随后，你会在议题的首条评论中看到一组 👀，表示 Copilot 正在处理任务！

![Copilot 使用眼睛表情符号表示它正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在认真处理第二项请求！Copilot cloud agent 的工作方式类似软件工程师，因此不必主动监控它，等任务完成后审查即可。现在开始创建和使用自定义智能体。

## 总结与后续步骤

本课探索了 [GitHub Copilot cloud agent][copilot-agents]。借助 cloud agent，可以将议题分配给 Copilot 异步处理。Copilot 可用于处理技术债务、创建新功能，或帮助将代码从一个框架迁移到另一个框架。

本课中，你了解了：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 清晰界定议题范围的重要性。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台工作，现在可以开始创建并使用自定义智能体。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并且可以使用自定义说明；这些内容已经在前面的模块中介绍过。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
