---
title: "课程 5 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能、自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不仅仅是写代码。我们已手动验证代码可以运行，也使用说明文件确保代码遵循标准。但测试、lint 和其他持续集成 (CI) 任务怎么办？

这类任务最适合使用**智能体技能**！技能可以帮助 Copilot 正确执行这些操作。

本课将：

- 探索现有的 `quality-checks` 技能。
- 自定义结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和支持资源打包在一起，供智能体按需加载。技能本质上是一个文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。Markdown 包含定义技能名称和说明的 frontmatter、技能功能概述，以及何时调用技能的指导。技能文件夹还可以包含脚本和其他资源子目录，供技能执行时使用。

> [!NOTE]
> 技能不一定需要额外的文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以存放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以存放在用户技能目录 `~/.copilot/skills` 中。

## 探索技能

来看看 Tailspin Toys 团队创建的 `quality-checks` 技能，它用于运行单元测试、lint 和类型检查。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。说明会帮助 Copilot 判断何时调用此技能。
3. 阅读指令，留意它如何引导 Copilot 执行测试和 lint。
4. 注意技能中目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 或使用自然语言调用技能。现在请 Copilot 运行技能中的三项检查。

1. 返回 Interactive 模式下的筛选功能对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后的报告。

## 自定义报告

现在改进报告，使其说明运行了哪些检查、是否成功，以及工具实际报告的内容。接下来更新技能以生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

完成更改后，看看实际效果！Copilot CLI 可以在不重启对话的情况下重新加载修改过的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后的报告并与第一个报告比较。

## 总结与后续步骤

你已自定义并使用现有智能体技能。本课中，你：

- 探索了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

这些更改会与筛选功能一起进入功能 PR。接下来，让 Copilot 直接与网站交互，[使用 Playwright MCP 服务器][next-lesson]。

## 更多技能示例

以下社区示例供参考，不是额外的练习任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
