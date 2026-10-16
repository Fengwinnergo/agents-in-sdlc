---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索存储库说明、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成式 AI 的使用离不开上下文。任务需要按特定方式完成时，应确保 Copilot 能获取相关指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。筛选功能已经构建完成，现在来探索 Copilot 使用的说明、添加文档标准并将其应用于代码。

本课将：

- 探索存储库说明和按路径限定范围的说明如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是重复代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果存在可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持准确。
- 保留现有格式和 lint 指南。

通过说明文件，可以确保 Copilot 获得所需信息，并按照这些实践完成任务。

## 说明文件

自定义说明可为 Copilot 提供上下文和偏好，让它更好地了解你的代码风格和要求。可以指定首选编码约定、库，以及代码注释的编写偏好等。可以为整个存储库创建说明，也可以为特定文件类型或任务创建说明。

说明文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送的单个说明文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南的说明文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。依赖某种格式前，请先查阅[自定义说明支持参考][custom-instructions-support]。

## 探索项目中的自定义说明文件

起始项目已包含一组说明文件。修改之前先看看现有内容，了解这些文件如何发挥作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（不是终端中）打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指南。此文件中的说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。这里包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` frontmatter；它会指定说明适用的文件 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和现有项目文件。这样可以将较大的说明集拆分为较小、可复用的文件，并指向可供 Copilot 参考的示例。

## 更新说明文件以符合团队指南

现有文件已经打下了良好基础，但还有不足。现在修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，应该在第 35 行附近。
2. 在该部分最后添加以下条目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指南

Copilot CLI 会在对话启动时加载存储库说明。保存更新后的说明后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选功能对话仍处于打开状态，请输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你已探索 Copilot 如何读取说明文件中的上下文，并将新标准应用于功能。本课中，你：

- 探索了存储库说明和按路径限定范围的说明如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 了解了说明文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，以确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
