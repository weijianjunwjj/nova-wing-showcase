# Spec-driven Development

NovaWing 的起点不是“选一个模型然后开始 Prompt”，而是先把研发目标变成 **可以被执行、被约束、被验收的规格**。

这也是 Spec Kit 在 NovaWing 中最重要的作用。

---

## From vague request to executable spec

真实需求通常一开始是模糊的：

> “把中断恢复做完整一点。”

如果直接交给编码 Agent，它需要自己猜：

- 什么叫“完整”？
- 哪些状态必须保存？
- 原有行为能不能改？
- 中断后从哪里继续？
- 哪些副作用不能重复？
- 什么测试通过才算完成？

NovaWing 更希望先得到类似这样的结构：

```text
Requirement
  ↓
Specification
  ├─ Goal
  ├─ Constraints
  ├─ Acceptance Criteria
  ├─ Do Not Do
  └─ Scope / Boundaries
  ↓
Task 01–99
  ↓
Brain / Executor workflow
```

规格层负责减少“靠 Agent 自己猜”的空间。

---

## What gets fixed in the specification?

公开层面可以把规格理解成四组信息。

### Goal

最终要解决什么问题。

不是“改几个文件”，而是期望得到什么工程结果。

### Constraints

实现过程中必须保持什么条件。

例如兼容性、现有状态语义、依赖边界、性能或环境要求。

### Acceptance Criteria

什么证据出现以后，任务才有资格被认为完成。

它是 Review 和 Verification 的共同锚点。

### Do Not Do / Boundaries

明确哪些事情即使“看起来顺手”也不能做。

这对于 Agent 尤其重要，因为强执行能力如果缺少边界，很容易把局部任务扩展成大范围修改。

---

## Spec Kit + Task 01–99

在 NovaWing 中，Spec Kit 和 Task 01–99 并不是同一层。

可以简单理解成：

```text
Spec Kit
= 定义这次工程到底要造什么，以及什么叫造完

Task 01–99
= 把这份规格拆成一个个有限、可追踪、可验收的执行单元
```

因此 Task 不是从聊天上下文临时冒出来的 Todo，而应能追溯回上层规格。

---

## Why this matters for AI agents

传统开发里，很多隐含信息可以依靠团队成员的长期上下文补齐。

Agent 的问题是：

- Session 会结束；
- 模型可能切换；
- Executor 可能切换；
- 长上下文会衰减；
- 自然语言会产生歧义；
- 每一轮都重新“理解需求”可能发生漂移。

因此规格的价值不只是写文档，而是建立一个稳定参照物：

> **无论运行到第几轮、换成哪个 Executor，原始目标和验收边界都不应该跟着聊天漂移。**

---

## Specification is not micromanagement

规格驱动并不意味着把每一行代码提前写死。

NovaWing 希望固定的是：

```text
目标
边界
约束
验收标准
风险规则
```

而把：

```text
具体实现选择
局部代码组织
合理重构方式
测试实现细节
```

留给 Brain / Executor 在任务边界内做工程判断。

也就是说：

> **规格约束结果与边界，而不是把 Agent 降级成机械脚本。**

---

## The long-term idea

当代码生成越来越便宜以后，真正稀缺的会越来越偏向：

- 定义正确的问题；
- 把模糊目标变成明确规格；
- 给执行系统建立责任边界；
- 用可靠证据验收结果。

因此 NovaWing 把 Spec-driven development 放在整个 Agent 工作流的最上游，而不是把它当作一个可有可无的文档步骤。
