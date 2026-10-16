---
title: "课程 2 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 对游戏卡片进行小幅更改，在转发的浏览器中审查结果，并合并为第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过一次对话后，现在可以开始对项目进行第一次更改了。先从简单的任务入手：游戏数据中已经包含星级评分，但首页上的游戏卡片还没有显示评分。请智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

本课将：

- 在功能分支上启动一个专注的 Copilot 对话。
- 请智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示评分。不过，首页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，请智能体在每张卡片上显示现有评分——这项更改很小、独立，非常适合第一次会话。

## 对话的组成

**对话**是你使用 Copilot CLI 执行任务的地方。与 Copilot app 不同，常规 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可让你稍后回到同一讨论，同时文件和分支仍是磁盘上的普通 Git 状态。

对话中会显示三类内容：你的提示和智能体响应、智能体探索和编辑文件时执行的工具操作，以及可以用 `/diff` 检查的更改。

## 开始对话并提出更改请求

启动一个新对话，开始实现功能。

1. 返回 Codespace。
2. 如果终端没有打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot CLI 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 启动新会话，然后按 <kbd>Enter</kbd>。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、找到显示游戏详情所用的文件，并创建必要的代码。至此，你已使用 Copilot CLI 添加了新功能！

## 审查差异

合并前必须审查所有 AI 生成的更改，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改文件。
2. 确认游戏卡片会在有评分时显示数值评分；当 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 执行的检查结果；如果有失败，请让它修复。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性，并非每次都相同。请审查行为，不要要求实现必须与某个固定版本完全一致。

## 检查更改

不能只阅读代码就假定一切正常。请让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用后，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五颗星的评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成！现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查已更改文件和检查结果。
5. 确认可以合并后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地的 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你使用 GitHub Copilot CLI 发布了第一个更改！具体来说，你：

- 在功能分支上启动了一个专注的 Copilot 对话。
- 指示智能体对游戏卡片进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，你将[从筛选议题开始，并使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
