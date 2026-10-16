---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用 AI 助手。你可以通过存储库中的 `.github/agents` 文件夹内的 Markdown 文件定义智能体，为 Copilot 提供专注的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可将专业知识编码为可复用的智能体，例如负责执行 [WCAG][wcag] 合规检查的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局位置 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段；之后的 Markdown 提示用于定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]在概念上有一些重叠。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最容易理解的区别是：**自定义智能体**负责工作，**技能**提供工具。

自定义智能体有自己的上下文窗口，可在工作时协调技能（甚至其他智能体）。在本实验中，无障碍自定义智能体会依据无障碍指南审查并更新网站；在此过程中，它可以调用拉取请求工作流技能或运行、管理测试的技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。与 AI 中的其他功能一样，应通过测试和迭代找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论他们的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前的暗色主题难以阅读，因为文本和背景颜色的对比度不足。为解决无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此需要尽快实现该功能。你将使用自定义智能体生成此功能。

本练习将：

- 审查现有的无障碍自定义智能体。
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 审查无障碍自定义智能体

已为你准备好无障碍自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 创建一个新分支来处理无障碍功能（将练习 3 中的筛选功能 PR 保持为独立更改）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中概述的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，聊天会话就会遵循该智能体的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，启动干净的对话。不同练习使用的模式和模型可能不同，相关步骤会注明需要注意的选项。

1. 如果 Chat 视图智能体下拉菜单中尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的智能体选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择聊天视图底部的智能体下拉菜单（其中显示当前智能体；默认情况下为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会开始工作——探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中以内联形式显示，并附有文件路径和差异供你审查。
6. 随着编辑应用，**Files changed** 指示器会更新，以显示智能体更改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 正在实际更改存储库，会根据需要编辑 Astro 组件、CSS 和相关测试等现有文件。

下一练习中，你将审查并引导这项正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即面向特定任务和领域的专用 AI 助手。自定义智能体可将团队的专业知识和标准转化为可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课中，你了解了：

- 自定义智能体的定义方式。
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，审查进行中的更改，并在同一会话中添加浅色模式切换。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
