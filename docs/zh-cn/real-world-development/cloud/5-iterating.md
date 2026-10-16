---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查成果

在整个实验中，你与 GitHub Copilot 合作完成了多项任务，旨在改善用户体验并添加功能：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在一起查看代码更改；如有需要，也可以向 Copilot 提供反馈以改进成果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍然需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用审查之前，先审查 GitHub Copilot 创建的文档、相关游戏功能和无障碍功能。

## GitHub Copilot cloud agent 与安全

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 对存储库只有读取权限，对其代码分支以外的内容没有写入权限。
- Cloud agent 在 GitHub Actions 中运行，并在其中创建独立的临时环境来处理任务。
- 任何 GitHub Actions 工作流都必须经人工批准才能运行。
- [默认情况下，外部资源访问受限][agent-firewall]，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求 (PR)，即为代码添加文档。此任务将通过 GitHub.com 标准 PR 界面完成。

> [!NOTE]
> 查看 PR 时可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests** 打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。可以趁此休息，或回顾目前所学的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 浏览更新后的代码，其中包含新建的 TSDoc 文档注释和其他文档。具体更改可能有所不同。

   检查更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些规则来自你在练习 2 开始时查看过的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR 检查区域会显示已排队的工作流。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。

## 向 GitHub Copilot 请求更改

与 Copilot 协作并不是单向过程。可以像标记团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。Copilot 会看到这些评论，并启动新会话来处理它们。由于生成式结果并非确定性的，无法给出固定的提示文本。可以请求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释标题。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 添加说明 Astro 应用结构的 README。

1. 添加评论请求 Copilot 修改生成的文档，并像标记其他用户一样标记 **@copilot**。可以使用上面的建议，也可以提出其他文档修改意见。
2. 选择 **View Session**，观察 Copilot 如何执行更改。Copilot 会启动新会话来更新文件。
3. 可以选择 **Back to pull request** 返回 PR。

   ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

如果需要，可以继续迭代，直到满意为止。准备好后，可将 PR 从草稿转换为可审查状态，然后合并到 `main` 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在回到 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR 检查区域会显示已排队的工作流。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已经构建了相关游戏功能！与之前一样，可以继续迭代，请 Copilot 更新成果。例如，可以要求调整显示的相关游戏数量，或添加注释标题和 TSDoc 文档注释（记住，分配该议题时，你还没有更新自定义说明）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应同时包含你在练习 3 中分配的高对比度模式，以及你在练习 4 的智能体控制台中请求添加的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，重点关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR 检查区域会显示已排队的工作流。如果一切正常，单个 Astro 应用的项目检查应该会通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace，启动终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能正常且在重新加载页面后仍能保留设置。

注意，无障碍自定义智能体如何帮助 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进机会，可以像处理前几个 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

Cloud agent 最适合处理真实待办事项，让你可以转而处理其他工作。可以在存储库中创建一些议题并分配给 Copilot，练习这种工作方式。以下是一些示例：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 环境路径。通过这些课程，你：

- **检查了存储库附带的自定义说明文件**，了解它们如何影响 cloud agent 的输出。
- **向 Copilot cloud agent 分配了议题**，并观察它如何设置环境、规划和异步执行任务。
- **创建并使用了无障碍自定义智能体**，添加高对比度和浅色模式切换。
- **将 Copilot agents 页面用作控制台**，在任务进行中监控并引导无障碍会话。
- **审查并迭代了 cloud agent 的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成 Cloud agent 环境路径。如果希望继续学习，其他环境路径可以补充本次练习的内容：

- 🖥️ **[VS Code 环境路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境路径](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` cloud agent 桥接功能，完成并审查两项更改。

在自己的存储库中，可以尝试以下后续练习：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 针对不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
