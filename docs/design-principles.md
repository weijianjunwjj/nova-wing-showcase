# Design Principles

NovaWing Development Guardian 的设计原则来自一个很现实的前提：

> **模型能力越强，越不能把“能做”误认为“应该被允许做”。**

因此 NovaWing 关注的重点并不是单次生成质量，而是复杂研发任务在长链路中的可靠性、边界和可恢复性。

## 1. 规格优先，而不是 Prompt 优先

Prompt 可以临时描述任务，但复杂工程更需要稳定的任务事实。

NovaWing 更关注：

- Goal 是否清晰；
- Constraints 是否明确；
- Acceptance Criteria 是否可检查；
- Scope 是否有限；
- Do Not Do 是否明确；
- Verification 是否可执行。

模型可以参与理解这些内容，但不能随意改写它们。

---

## 2. 决策与执行分离

让同一个 Agent 同时决定标准、执行修改、再自行验收，会形成明显的角色冲突。

NovaWing 采用 Brain / Executor 分层：

- Brain：理解、判断、审查；
- Executor：实现、测试、修改。

这不是为了“多用几个模型”，而是为了形成更清晰的责任边界。

---

## 3. 报告不等于事实

LLM 很擅长生成流畅、完整、听起来可信的总结。

但研发系统真正需要的是证据。

因此 NovaWing 把 Executor 的自然语言报告视为 **untrusted execution evidence**，而不是新的任务指令或最终事实。

判断任务是否完成时，更看重：

- 实际 changed files；
- 实际 command results；
- deterministic verification；
- repository state；
- acceptance criteria coverage。

---

## 4. 硬边界不能被自然语言软化

如果任务规定：

```text
Allowed Paths
Forbidden Paths
Constraints
Do Not Do
```

这些内容就是边界，不应该因为 Executor 提出一个“看起来合理”的理由而自动失效。

越强的 Agent，越需要稳定的权限与范围控制。

---

## 5. 自动化必须允许“停下来”

一个可靠 Agent 系统不应该只会继续执行。

它还必须知道什么时候：

- blocked；
- suspended；
- human required；
- failed safely。

能够安全停止，本身就是自动化能力的一部分。

---

## 6. 人类审批不是失败，而是控制面

Human-in-the-loop 不等于“AI 不够聪明”。

真正需要人工参与的往往不是代码语法，而是：

- 权限；
- 风险；
- 产品取舍；
- 不可逆动作；
- 业务责任。

NovaWing 希望把人类从重复执行中移出，但保留在真正需要承担责任的决策点上。

---

## 7. 验证优先于自我评价

“我检查过了，应该没问题”是语言输出。

`tests passed`、`typecheck passed`、`build passed` 才更接近工程证据。

NovaWing 倾向把验证设计成可重复、确定性的动作，并把结果重新交给 Reviewer 判断。

---

## 8. 恢复是长任务的一等能力

AI Coding 很容易在短任务里显得强大。

真正困难的是一个持续数十分钟甚至更久、经历多轮修改和验证的任务。

因此 NovaWing 把：

- checkpoint；
- session state；
- continuation；
- recovery；

当作核心工程问题，而不是事后补丁。

---

## 9. 保留可信进度，而不是重复劳动

恢复时最糟糕的做法之一，是让 Agent 每次重新分析整个仓库并从头开始。

一个好的恢复机制应该尽可能：

- 识别已完成部分；
- 保留有效修改；
- 确认可信 checkpoint；
- 只继续剩余工作；
- 再次执行必要验证。

这会直接影响 Agent 在真实大型项目中的成本、稳定性和可用性。

---

## 10. 模型可替换，工程机制更重要

NovaWing 不希望核心价值绑定在某一个模型名称上。

今天可以是 GPT + Claude Code / Codex，未来执行层也可以替换为其他 Agent。

真正希望保留下来的，是：

> **Specification → Decision → Execution → Evidence → Verification → Human Control → Recovery**

这条研发控制链。

模型会快速变化，工程责任不会消失。
