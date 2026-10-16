---
title: "课程 7 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，也通过 Playwright MCP 在浏览器中观察了筛选体验。现在将这些能力整合到一个具有明确 QA 流程的自定义智能体中。

本课将：

- 探索自定义智能体如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件会说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的各类自定义功能承担不同职责：存储库说明描述团队标准，`quality-checks` 技能封装可重复的检查，Playwright MCP 提供浏览器工具。QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些功能，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查，以及必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，请先审查其配置文件，确认 Copilot 记录了预期的 QA 工作流和边界。这样可以避免配置不完整或范围过宽的智能体在你只想验证功能时修改代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需项；`name` 是可选项，但填写后更容易识别智能体。
3. 阅读配置文件说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了任何职责或边界，请先让默认智能体修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选功能对话，再选择 QA，让它使用已有的议题和规划决策。

1. 使用以下提示启用该智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于你刚创建了该智能体，它可能不会显示在可用智能体列表中，但上述命令仍可启用它。

2. 使用以下提示，让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后阅读它提供的报告。

## 总结与后续步骤

你已为工作流添加可复用的专用角色，并审查了它的工作。本课中，你：

- 探索了自定义智能体如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其发现。

现在，已有实现、技能更新、QA 配置文件、测试和验证报告，可以进行审查。接下来，[将它们整合到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
