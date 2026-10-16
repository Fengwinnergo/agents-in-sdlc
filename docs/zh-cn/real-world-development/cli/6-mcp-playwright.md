---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据、外部服务，并让 Copilot 使用更多自动化工具。这正是 MCP 服务器发挥作用的地方。MCP 服务器可扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

本课将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 让智能体驱动浏览器并体验筛选功能。

## 场景

单元测试和端到端测试固然重要，但要验证 UI 更新，仍需像用户一样实际操作 UI。通过允许 Copilot 使用你正在开发的网站，可以进一步自动化更改验证，并增强对功能行为符合预期的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体与外部工具和服务提供通信方式。借助 MCP，AI 智能体可以实时与外部工具和服务通信，从而获取最新信息并代你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁。每个 MCP 服务器都提供一组智能体可以访问的工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：提供基于 Playwright 的浏览器自动化功能。

此外还有很多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，方便发现相关项目并为生态系统作出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果已在 Copilot CLI 中配置 Playwright MCP，请跳过设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 通过 Playwright 探索功能

之前你已手动确认功能行为符合预期。现在使用 Copilot 中的 Playwright MCP 服务器执行相同检查！

1. 使用以下提示，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，因此不一定非要明确指定。不过，指出应使用的工具有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行各项操作，确认功能正常。
3. 阅读报告并确认行为符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后提供报告。

## 总结与后续步骤

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器在真实浏览器中体验了功能！本课中，你：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果此前未配置，则添加了 Playwright MCP 服务器。
- 让智能体驱动浏览器并体验筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门的角色中。

## 资源

- [什么是 MCP？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
