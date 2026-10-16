---
title: "课程 10 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装了 Copilot CLI、探索了项目，并找到已创建的筛选议题。
- 添加了星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选功能更改，并授权 Agent Merge 合并筛选功能 PR。
- 探索了用于管理上下文、模型、共享和可选云委派的斜杠命令。

## 发布的内容

本工作坊包含两个 PR 里程碑；每个 PR 都从更新后的 `main` 分支创建自己的分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：**实现筛选功能、更新说明并将说明应用于该功能、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到打开 PR，你使用了同一个对话和分支。为了简化工作坊流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自使用浏览器检查，以及通过 MCP 让 Copilot 在浏览器中探索。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 会在 PR 前综合这些结果，并检查需求和测试覆盖范围。

新增测试应弥补真实缺口；QA 运行后无需新增测试也可能是正确结果。授权合并前，请审查代码和证据；代码更改后应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可在不同对话中复用这些自定义项，并按团队需要进行调整。说明用于设置标准，技能描述可重复的任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择合适的**模式和模型**。使用 **Plan** 先确定方案，使用 **Interactive** 参与范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。例行编辑可选用较快的模型，复杂工作则使用能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建*什么*、*为什么*以及*如何*实现，会显著影响输出结果。

## 可以继续探索

已完成核心工作流。还可以了解以下 CLI 功能：

- 使用 `/review` 让代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

提升任何工具使用能力的最佳方式就是持续使用！将它用于生产代码、个人项目，或那个想了很多年却一直没开始实现的小应用。与团队分享经验，也向团队学习。一如既往，记得查阅文档。

如果想通过终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。要比较其他环境，也可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
