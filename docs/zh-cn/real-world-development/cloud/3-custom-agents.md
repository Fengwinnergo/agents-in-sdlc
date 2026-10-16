---
title: "练习 3 - 自定义智能体"
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

使用自定义智能体时，你将了解：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论他们的视觉能力或偏好如何。近期用户反馈指出，有些用户觉得当前的暗色主题难以阅读，因为文本和背景颜色的对比度不足。为解决无障碍问题，设计团队要求实现可由用户切换的高对比度模式。

无障碍至关重要，因此需要尽快实现该功能。你将使用自定义智能体生成此功能。

## 审查无障碍自定义智能体

已为你准备好无障碍自定义智能体。先审查其内容，了解它将如何引导 Copilot。

返回 Codespace，然后审查无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续章节，了解其中概述的内容：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见问题和错误列表。

## 创建并分配议题

智能体控制台是管理环境中所有智能体的中心位置。你可以将任务分配给 Copilot cloud agent、监控任务，甚至在任务进行中重新引导并提供补充信息。现在先将创建高对比度模式的任务分配给 Copilot。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从列表中选择 **Accessibility agent** 自定义智能体。

   ![云智能体分配界面截图，其中高亮了自定义智能体和无障碍选项](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，即面向特定任务和领域的专用 AI 助手。自定义智能体可将团队的专业知识和标准转化为可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课中，你了解了：

- 自定义智能体的定义方式。
- 如何将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。现在可以前往[智能体控制台监控并引导会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
