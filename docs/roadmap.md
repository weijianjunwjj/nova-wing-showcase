# Public Roadmap

NovaWing 的路线图不以“支持更多模型”作为唯一目标，而是围绕一个问题持续演进：

> **怎样让 AI 在真实研发任务里更可控、更可靠、更容易被验证和恢复。**

本页只描述公开安全的方向，不披露内部优先级、专利相关实现或未公开协议。

---

## Phase A · Reliable single-task loop

目标：先把单个复杂 Task 做成可靠闭环。

重点能力：

- Spec-driven task contract
- Brain / Executor separation
- execution evidence
- deterministic verification
- path / scope boundaries
- iterative review
- explicit completed / blocked / human-required states

这部分是 Guardian 的地基。

---

## Phase B · Long-running work sessions

目标：让任务不是只能“一次跑完”，而是能经历多轮执行和真实中断。

重点能力：

- Work Session lifecycle
- iteration / phase visibility
- suspended state
- approval continuation
- checkpoint / recovery
- recent activity / verification visibility
- restart-safe session semantics

核心问题从：

> “Agent 会不会写？”

变成：

> **“任务跑很久、途中出问题以后还能不能继续？”**

---

## Phase C · Pluggable executors

目标：让 Brain 不绑定某一个执行 Agent。

当前方向包括：

- Claude Code Executor
- Codex Executor
- future executor adapters
- stable delegation / evidence contract

原则是：

> **Executor 可以替换，但任务事实、验证规则和决策边界不能随着工具切换而消失。**

---

## Phase D · Stronger governance

目标：进一步减少 AI 研发中的隐式权限和模糊状态。

公开方向：

- richer approval policies
- risk-aware actions
- stronger repository checkpoints
- clearer side-effect boundaries
- auditable decisions
- safer continuation semantics

这里关注的不是让 Agent “更自由”，而是让高能力 Agent 在更明确的责任边界里工作。

---

## Phase E · Cross-project engineering context

目标：在不破坏项目边界的前提下，让长期研发经验能够跨 Session / 项目沉淀。

方向包括：

- reusable engineering memory
- project-specific context boundaries
- decision history
- reusable skills / patterns
- context selection rather than unlimited context stuffing

重点不是“记住所有东西”，而是：

> **在正确的时候，把正确的历史经验交给正确的任务。**

---

## Phase F · Measurable engineering value

最终 NovaWing 是否有价值，不应该只看功能数量。

更重要的指标包括：

- Task completion reliability
- human intervention frequency
- recovery success rate
- verification failure detection
- out-of-scope modification prevention
- repeated-work reduction
- cost / latency per accepted task
- long-task consistency

未来公开 Showcase 会优先补充这些可验证证据，而不是只增加概念图。

---

## Non-goals

至少在当前阶段，NovaWing 不追求：

- 为了“Multi-Agent”而无限增加 Agent 数量；
- 取消所有人工控制；
- 用 LLM 替代测试、类型系统和 CI；
- 宣称任何任务都能完全自主完成；
- 把模型厂商绑定当成系统核心能力。

真正希望稳定下来的，是：

**Specification · Responsibility · Evidence · Verification · Recovery**
