# Technical Review Guide

这份文档写给第一次看到 NovaWing 的技术负责人、面试官或潜在合作方。

由于核心仓库保持私有，最合理的评估方式不是问“为什么不开源”，而是判断：

> **这里展示的系统设计是否解决了真实的软件工程问题，公开证据是否与这些设计一致。**

---

## 5-minute review path

如果只有 5 分钟，建议按这个顺序看：

1. [`README.md`](../README.md) — 先理解 NovaWing 是什么。
2. [`brain-executor.md`](brain-executor.md) — 看清 GPT 与 Claude Code / Codex 的职责分离。
3. [`task-contract.md`](task-contract.md) — 看任务为什么不是一句 Prompt。
4. [`verification-recovery.md`](verification-recovery.md) — 看系统如何处理“AI 自报完成”和中断恢复。
5. [`demo.md`](demo.md) — 看后续公开运行证据与截图。

---

## Questions worth asking

### Architecture

- 为什么要把 Brain 和 Executor 分开？
- Brain 的输入事实有哪些？哪些信息不能被 Executor 改写？
- 如果 Brain 判断错了，系统还有哪些约束层？
- 为什么不让一个 Agent 从需求一直做到最终验收？

### Task contract

- goal / constraints / acceptance criteria / do-not-do 分别解决什么问题？
- allowed paths / forbidden paths 为什么属于硬边界？
- Task 01–99 是单纯编号，还是研发任务契约的一部分？

### Verification

- 什么结果由 LLM Review，什么结果交给确定性工具？
- 如何避免“Executor 说测试通过”就被当成测试真的通过？
- Verification 失败后下一轮输入如何组织？

### Human-in-the-loop

- 什么情况下系统必须停下来找人？
- 人工输入如何恢复当前 Session？
- 人工批准是否会隐式扩大 Executor 权限？

### Recovery

- checkpoint 代表什么样的可信进度？
- 中断后为什么不直接重新开一个 Agent？
- 如何避免恢复过程重复执行已有修改？

这些问题比“用了几个模型”“Prompt 有多少行”更能判断系统深度。

---

## What would be a red flag?

如果一个类似项目出现下面这些特征，需要谨慎：

- 只有架构图，没有真实执行证据；
- 所有状态都由模型自然语言自己宣布；
- 没有验收标准，只看 Agent 是否说 done；
- Agent 可以随意执行任何命令和修改任何路径；
- 失败后只能重新开始；
- Human approval 只是 UI 上一个按钮，没有明确 continuation 语义；
- 多 Agent 只是为了“看起来更高级”，没有清晰职责边界。

NovaWing 的设计恰恰是在持续消除这些问题。

---

## What this showcase can prove

公开仓库能够帮助验证：

- 系统是否有清晰的工程问题定义；
- 架构是否围绕规格、执行、证据、验证和恢复形成闭环；
- Brain / Executor 是否具有明确职责边界；
- 设计是否考虑真实失败路径，而不只有 Happy Path；
- 作者是否理解 AI Coding 从“工具使用”走向“工程系统”时出现的新问题。

公开仓库不能也不会证明：

- 私有源码的每一行实现质量；
- 尚未公开的专利 / IP 细节；
- 未展示场景下的生产级可靠性；
- 尚未完成的未来能力。

这也是为什么 [`implementation-status.md`](implementation-status.md) 会主动区分 Implemented、Evolving 与 Planned。

---

## Core evaluation idea

NovaWing 最值得评估的不是：

> “AI 能不能写代码？”

而是：

> **“当 AI 已经很会写代码以后，我们怎样让它在复杂软件工程里长期、受控、可验证地工作？”**

这才是 Development Guardian 想回答的问题。
