# Technical FAQ

## Why is GPT the Brain instead of another Executor?

NovaWing 并不假设某个模型天然“更高级”。Brain 是一个**职责角色**：负责理解任务、做下一步判断、审查证据，而不是直接操作仓库。

当前架构使用 GPT 承担 Brain / Reviewer，Claude Code / Codex 承担 Executor，是为了让决策与执行分离。未来模型可以替换，但角色边界希望保持稳定。

## Why not let Claude Code / Codex finish the whole task directly?

直接使用编码 Agent 对短任务非常有效。

NovaWing 关注的是更长、更复杂、更需要控制的任务：多轮修改、多个验收标准、权限边界、失败恢复、人工审批等。

问题从“模型会不会写代码”变成了：

> 谁定义完成？谁验证结果？谁决定继续？中断后怎么恢复？

这正是 Guardian 层存在的原因。

## What does Spec Kit add?

Spec Kit 在 NovaWing 中承担 specification 入口。

它帮助把需求从聊天式描述转成更稳定的：

- requirements；
- constraints；
- acceptance criteria；
- design / implementation context。

NovaWing 再把规格映射到 Task 01–99 的执行链路中。

## Why Task 01–99?

`01–99` 本身不是算法，也不是限制项目只能有 99 个动作。

它是一种稳定的研发任务编号约定，让人、Brain 和 Executor 能够围绕同一个任务单元讨论：

- 当前做到哪；
- 哪个任务已验收；
- 哪个任务需要恢复；
- 哪个任务引入了新的决策。

它把连续聊天转换成更接近工程项目管理的执行结构。

## Can the Brain trust the Executor report?

不能无条件信任。

Executor Report 是重要输入，但它属于 execution evidence，而不是新的权限来源或任务事实。

NovaWing 会把它与原任务约束、真实工程状态和 deterministic verification 一起审查。

## Why is deterministic verification important if the Brain can review code?

因为 Reviewer 仍然是语言模型。

语言模型 Review 很有价值，但无法替代所有可执行检查。能用 test / typecheck / build 明确验证的事情，没有必要只靠模型“感觉正确”。

更合理的组合是：

> **LLM judgment + machine-verifiable evidence**

而不是二选一。

## What happens when a task requires a dangerous or irreversible action?

系统应该进入人工控制，而不是因为“完成任务”而自动扩大权限。

Human Approval 是研发控制面的一部分。

公开 Showcase 不披露具体内部风险规则和授权协议。

## What happens if an Agent session is interrupted?

NovaWing 的方向是基于可信 checkpoint 继续已有任务，保留已经验证的进度，并再次校验恢复后的状态。

Recovery 不是简单地把旧 Prompt 再发一次。

具体 checkpoint schema 和恢复策略保持私有。

## Is this a multi-agent framework?

可以从广义上这样理解，但 NovaWing 更强调 **software engineering orchestration / control plane**，而不是追求 Agent 数量。

如果一个问题一个 Brain + 一个 Executor 就足够，就没有必要为了“Multi-Agent”标签增加更多角色。

## Is NovaWing an IDE or editor plugin?

不是核心定位。

IDE / Control UI 可以成为交互入口，但 NovaWing 的核心价值位于任务规格、决策、执行、验证、审批和恢复之间的控制链路。

## Is the project open source?

核心实现不是。

本仓库只公开 Showcase 内容。核心代码保持私有，原因包括持续研发以及后续知识产权 / 专利规划。

## How can a recruiter or interviewer verify the project without source code?

推荐通过四类证据：

1. 完整架构解释；
2. 真实运行 Demo / 截图；
3. Task lifecycle 与 verification 记录；
4. 面试中对具体 failure case、trade-off 与设计选择的深入讨论。

一个真实设计并实现过系统的人，通常能够解释“为什么这么做、哪里失败过、为什么不能简单换一种做法”，这比展示大量代码更能说明工程深度。
