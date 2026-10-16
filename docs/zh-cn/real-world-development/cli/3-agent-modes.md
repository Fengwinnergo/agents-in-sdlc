---
title: "课程 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案，使用 Autopilot 根据议题构建筛选功能，然后在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的内容。本课程将从议题出发生成一项新功能，确保代码有效、功能行为符合预期，最后成功合并到项目中。后续课程也会沿着这项功能继续开展。

> [!NOTE]
> 在后续功能工作中，你会继续使用同一对话和分支。通常，不同类型的文件会放在不同分支或 PR 中处理；本工作坊为了聚焦核心概念，会采用简化流程。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 在 Autopilot 模式中实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

后续课程会继续更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办议题描述了这项功能，但例如如何组合类别等细节需要在编码前确定。你将先在 Plan 模式中解决这些问题，然后授权 Autopilot 实现范围明确的方案。

## 背景

在开发工作流中引入 AI 编码智能体，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发人员遵循的流程大致如下：

1. 从已创建的议题开始，明确需要完成的工作。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码符合需求，并像手写代码一样经过审查。

## 对话模式

**对话模式**决定智能体拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并反复迭代，直到它认为任务已完成。

先在 Plan 模式中制定并审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选功能前，先返回 Codespace，确保存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在会以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以先确定功能实现方式或任务处理步骤，再交给 Copilot 执行。对于复杂任务，花些时间规划总是有益的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方式并制定计划。

3. Copilot 可能会询问筛选功能的行为细节。根据你的偏好回答；没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 浏览并审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已写好并审查完毕，现在开始实现！使用 Autopilot，让 Copilot 按计划工作。

Autopilot 会让 Copilot 反复处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，Copilot 可能会立即开始实现。因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再确认检查已完成。

## 审查并验证实现

代码生成后，与其他代码一样，合并前需要审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 根据议题和规划时作出的决定审查结果。
4. 代码审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目运行的检查结果；如有失败，请让 Copilot 修复。

## 体验新功能

代码看起来没问题，但实际运行效果如何？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用后，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中确定的组合方式。
4. 确认重置和无结果状态符合议题要求和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 在 Autopilot 模式中实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义说明][next-lesson]，进一步了解如何生成符合已记录实践的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
