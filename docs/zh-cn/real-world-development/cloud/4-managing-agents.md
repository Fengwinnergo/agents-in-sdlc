---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几次练习中，你让 Copilot cloud agent 承担了三项独立任务，旨在改善用户体验并添加功能。Cloud agent 可以异步且自主地工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制台中可以查看所有带有未关闭拉取请求 (PR) 的智能体任务，探索已执行的操作，甚至引导正在进行的会话。

本课将：

- 浏览智能体页面，监控 cloud agent 任务。
- 引导正在进行的会话，请求添加额外功能。

## 场景

将创建高对比度模式的任务交给智能体后，团队意识到还可以顺便添加浅色模式。既然正在更新网站样式并添加切换功能，加入此功能就很合理。你将引导智能体，使它在创建高对比度模式的同时也添加浅色模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。应该能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务。查看 Copilot 已执行的操作，其中可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会因 Copilot 当前的工作状态和所用方法而异。

4. 同时注意右侧的拉取请求 (PR) 窗格。可以在这里查看 PR 和已更改文件，以便进一步监控。

## 引导 cloud agent

既然已经查看了当前正在运行的任务，现在让 Copilot 在实现高对比度模式的同时也添加浅色模式。

1. 选择与高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

   ![智能体控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。此时会出现类似下面的消息。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![智能体页面中的 cloud agent 任务截图，其中高亮了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会开始处理更新后的任务！收到消息时，它会先完成当前步骤，然后将新请求纳入后续工作流程。

与之前一样，这可能需要几分钟。等待期间，不妨暂停一下，回顾目前学到和体验的内容。

## 总结与后续步骤

本课介绍了 Copilot agents 页面，这是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以用这个控制台跟踪所有活动和已完成的任务、审查正在执行的工作，甚至调整正在运行任务的范围或提供更多指导。

本课中，你：

- 浏览了智能体控制台和智能体页面，以监控 cloud agent 任务。
- 引导正在运行的会话，请求添加额外功能。

Copilot 正在完成无障碍功能。接下来[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
