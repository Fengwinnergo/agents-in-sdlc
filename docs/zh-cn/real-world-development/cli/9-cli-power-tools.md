---
title: "课程 9 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀 CLI 工具一样，GitHub Copilot CLI 也提供许多斜杠命令。这些命令可用于访问高级功能、查看内部信息以及配置选项。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。现在来了解其他一些实用命令。

本课将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云智能体的可选命令。

## 场景

CLI 核心工作流已完成。现在了解一些其他功能——管理上下文、切换模型、共享会话，以及选择是否将任务委派给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理规模较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；你也可以自行检查或使用斜杠命令压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 使用量，以及上下文如何划分为系统说明、工具、消息和可用空间。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口快满时自动压缩。想自行选择时机，可以使用 `/compact`。如果要切换到无关任务并重新开始，则使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发人员也有各自偏好的模型。Copilot CLI 支持列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于每个人改进 AI 工具的使用方式。`/share` 命令可将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中 `/share` 的选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，将会话导出为本地 Markdown 文件。
3. 发送或发布导出内容前先进行审查。会话导出可能包含提示、响应和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意公开的存储库或对话内容。

## 可选：扩展任务或委派工作

核心工作坊内容已完成。Copilot CLI 还提供适用于较大任务的命令：

- `/fleet` 可将独立子任务分配给多个子智能体并行执行。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，以异步方式处理，并可能创建拉取请求。

这些命令可能会创建额外的工作树或远程工作，因此属于可选内容。尝试前先创建一个范围明确的新任务，并按常规流程审查结果。如果希望进一步了解异步智能体工作流，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 斜杠命令可以进行配置、共享会话并了解内部运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 浏览可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了用于并行工作、工作树和委派给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令值得探索！接下来[回顾所学内容][next-lesson]，并了解后续学习步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [在 Copilot CLI 中管理上下文][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
