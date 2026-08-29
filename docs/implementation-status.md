# Implementation Status

本页只公开 **能力级别** 的实现状态，不公开核心源码、完整协议、Prompt 或关键控制策略。

> NovaWing Development Guardian 仍处于持续研发阶段。这里的状态用于帮助技术负责人理解：哪些是已经落地的工程能力，哪些仍在继续演进。

## Current capability matrix

| Capability | Status | Public description |
| --- | --- | --- |
| Spec-driven task input | ✅ Implemented | 任务由目标、约束、验收标准、禁止事项等结构化信息驱动 |
| Task 01–99 workflow | ✅ In use | 将复杂目标拆成编号化、可追踪、可验收任务 |
| Brain / Reviewer | ✅ Implemented | GPT 负责理解任务、判断下一步、Review 执行证据与关键决策 |
| Claude Code Executor | ✅ Implemented | 负责受控的代码修改、测试与工程执行 |
| Codex Executor | 🟡 Evolving | 作为可替换执行层持续接入与完善 |
| Execution evidence | ✅ Implemented | Executor 返回实际变更、命令、验证结果与 blocker 等执行证据 |
| Deterministic verification | ✅ Implemented | 通过 test / typecheck / build / lint / repository checks 等确定性结果进行验证 |
| Allowed / forbidden paths | ✅ Implemented | 将允许修改范围与禁止路径作为硬边界 |
| Iterative review loop | ✅ Implemented | Brain 可根据证据继续、要求修订、阻塞或完成任务 |
| Human-required state | ✅ Implemented | 无法安全自动决策时挂起并等待可信人工输入 |
| Approval continuation | ✅ Implemented | 人工决策后可继续既有 Session，而不是隐式扩大权限 |
| Checkpoint / Recovery | ✅ Implemented | 支持从已验证进度继续执行，避免无条件从头开始 |
| Work Session lifecycle | ✅ Implemented | 管理准备、执行、Review、验证、恢复等长任务阶段 |
| Control UI | ✅ Implemented | 展示阶段、状态、轮次、Recent Activity、验证与恢复信息 |
| Cross-project memory | 🟡 Evolving | 作为 NovaWing 更大范围能力持续演进 |
| Production-scale multi-agent scheduling | 🔵 Planned / Research | 不以“Agent 越多越好”为目标，只有职责真正需要时才扩展 |

状态说明：

- ✅ **Implemented**：已存在实际工程实现或正在真实任务中使用。
- 🟡 **Evolving**：已有能力基础，但接口、策略或覆盖范围仍在迭代。
- 🔵 **Planned / Research**：明确方向，但不把尚未完成的能力包装成已实现。

---

## What is intentionally not claimed

NovaWing 当前不会把下列内容包装成已经完成：

- “完全无人值守的软件公司”；
- 任意规模代码库都能自主开发；
- Brain 永远正确；
- Executor 永远不会越界或出错；
- 使用更多 Agent 就必然更可靠；
- 已经替代完整研发团队。

NovaWing 的目标更务实：

> **把 AI 编码能力放进一套有规格、有边界、有证据、有验证、可暂停、可恢复的工程闭环里。**

---

## How to evaluate progress

比起看“支持了多少模型”，更适合用下面几个问题判断 NovaWing 是否真的进步：

1. 一个复杂 Task 能否被规格化并稳定执行？
2. Executor 的完成声明能否被独立证据验证？
3. 越界或风险动作能否被阻止或转人工？
4. 中断后能否从可信状态继续？
5. 多轮执行是否仍然保持原始目标、约束和验收标准？
6. 新 Executor 接入时，是否能复用稳定的 Brain / Task / Verification 合约？

这也是 NovaWing 当前研发的主要评价框架。
