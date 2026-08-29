# Case Study · A Sanitized Task Lifecycle

> 本案例是根据 NovaWing 的真实工作方式整理出的 **公开安全示例**，用于解释系统如何处理一个复杂研发任务。它不包含私有仓库源码、完整 Prompt、真实路径或内部协议细节。

## Scenario

假设需要为 Guardian 的控制界面补充更清晰的执行状态展示，同时不能改变现有任务状态语义，也不能影响 Executor 的实际执行路径。

传统的一句话任务可能是：

> “把控制台状态显示优化一下。”

NovaWing 不会直接把这句话交给 Executor。

---

## Step 1 · Specification

先把目标变成可执行规格：

```yaml
goal: >
  让用户能够明确识别当前研发任务处于哪一个执行阶段，
  并看到 verification 与 recent activity 的关键状态。

constraints:
  - 保持现有任务状态机语义
  - 展示层不得改变 Executor 的执行流程
  - 不扩大到无关模块重构

acceptance_criteria:
  - 当前 phase / status 可被清晰识别
  - verification running / passed / failed 可区分
  - existing regression tests continue to pass

verification:
  - npm test
  - npm run typecheck
  - git diff --check
```

此时“优化一下”已经变成了可以验收的工程目标。

---

## Step 2 · Task Contract

该规格被拆成一个稳定 Task，例如：

```text
Task 27
├─ Goal
├─ Constraints
├─ Acceptance Criteria
├─ Allowed Paths
├─ Forbidden Paths
├─ Do Not Do
└─ Verification
```

Task ID 在多轮执行中保持稳定。

即使发生：

```text
iteration 1
→ review
→ iteration 2
→ verification
→ suspended
→ recovery continuation
```

所有角色仍然围绕同一个任务事实工作。

---

## Step 3 · Brain Decision

GPT Brain / Reviewer 读取：

- 原始 Task；
- 当前已有进度；
- 上一轮执行证据；
- verification 结果；
- 允许 / 禁止边界。

然后决定这一轮 Executor 应该做什么。

示意：

```text
Decision: proceed

This iteration:
1. update display mapping only;
2. preserve task-state semantics;
3. add focused regression tests;
4. run required deterministic verification.
```

Brain 不直接假装代码已经改完。

---

## Step 4 · Executor Action

Claude Code / Codex Executor 接收有限范围的任务委托，实际执行：

```text
Read repository
→ Edit allowed files
→ Update / add tests
→ Run permitted commands
→ Inspect diff
→ Return execution evidence
```

Executor 返回的重点不是一段漂亮总结，而是：

```text
changed files
commands run
verification results
blockers
incomplete reason (if any)
```

---

## Step 5 · Deterministic Verification

系统不会因为 Executor 写下：

> “功能已经完成，测试应该没有问题。”

就进入 Completed。

需要继续检查真实验证结果，例如：

```text
npm test           PASS
npm run typecheck  PASS
git diff --check   PASS
```

如果其中一项失败，失败本身会成为下一轮 Review 的输入证据。

---

## Step 6 · Brain Review

Brain 将执行结果重新与原始 Acceptance Criteria 对齐。

可能得到四类方向：

```text
proceed  → 继续下一步
revise   → 当前实现需要修订
blocked  → 存在无法自动解决的 blocker
done     → 证据足够，任务验收完成
```

重点是：

> **Brain 的 Review 锚点始终是原始 Task，而不是 Executor 后来如何描述任务。**

---

## Step 7 · If interruption happens

如果在多轮任务中出现中断，系统优先判断是否存在可信进度。

```text
Existing verified changes
        ↓
Checkpoint / continuation decision
        ↓
Resume existing work
        ↓
Finish remaining verification / implementation
```

恢复提示的核心语义是：

```text
保留已有修改
不要重新从头实现
只继续剩余工作
原始约束仍然有效
```

---

## Step 8 · If human input is required

如果出现系统无法安全自动裁决的决策：

```text
Brain → HUMAN_REQUIRED
        ↓
Session suspended
        ↓
Human provides explicit continuation input
        ↓
Resume under original task boundaries
```

人工输入解决当前决策，但不会自动让 forbidden paths、权限或验收标准失效。

---

## Final result

一个 Task 真正完成时，链路更接近：

```text
Specification
✓
Task Contract
✓
Brain Decision
✓
Executor Action
✓
Execution Evidence
✓
Deterministic Verification
✓
Brain Review
✓
Accepted
```

这就是 NovaWing 所说的“研发闭环”。

它追求的不是：

> **AI 一次写对所有代码。**

而是：

> **即使任务需要多轮、会失败、会中断，也仍然能围绕同一规格被控制、验证并继续推进。**
