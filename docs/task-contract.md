# Task Contract

`Task 01–99` 不是一个简单的 Todo 编号系统。

它的核心价值，是把每个研发任务变成 Brain、Executor 和 Human 都能共同理解的 **执行契约**。

## Public task shape

下面是经过简化、适合公开展示的 Task Contract 结构：

```yaml
id: task-27

goal: >
  为控制界面增加当前执行阶段与验证状态展示。

constraints:
  - 不改变既有任务状态语义
  - 保持现有 API 向后兼容
  - 新增展示逻辑不得影响 Executor 执行路径

acceptance_criteria:
  - 当前阶段可被用户明确识别
  - verification running / passed / failed 可区分
  - 已有回归测试继续通过

allowed_paths:
  - packages/development-guardian/work-session/control-ui/**
  - tests/**

forbidden_paths:
  - packages/core/**
  - release/**

do_not_do:
  - 不通过删除测试来获得通过结果
  - 不扩大任务之外的重构范围

verification:
  - npm test
  - npm run typecheck
  - git diff --check
```

> 以上内容是公开示例，不代表私有仓库中的完整内部 schema。

---

## Why contract instead of prompt?

如果只告诉 Agent：

> “把控制台优化一下。”

它会面对大量隐含选择：

- 优化什么？
- 可以改多大范围？
- 能不能顺便重构？
- 什么才算完成？
- 测试失败能不能先忽略？

Task Contract 把这些默认假设显式化。

这会让 Agent 的工作模式从：

> **“根据上下文猜一个合理实现。”**

变成：

> **“在明确目标、边界和验收条件下完成一个有限工程任务。”**

---

## Stable task identity

Task 编号也承担稳定索引作用。

```text
Spec
├─ Task 01
├─ Task 02
├─ Task 03
│   ├─ iteration 1
│   ├─ iteration 2
│   └─ recovered continuation
├─ ...
└─ Task 99
```

当任务经历多轮执行、Review、暂停和恢复时，所有角色仍然围绕同一个 Task ID 工作。

这比依赖聊天窗口里的“上一轮我们做到哪里了”更接近真正的软件工程流程。

---

## What the Brain may decide

Brain / Reviewer 不应任意重写任务事实。

它可以根据证据决定：

```text
继续执行
修订当前实现
要求补充验证
因为 blocker 停止
请求人工决策
从可信 checkpoint 恢复
验收完成
```

但不应该擅自把：

```text
Acceptance Criteria
Forbidden Paths
Do Not Do
```

变成可选建议。

---

## What the Executor owns

Executor 对“真实执行”负责：

```text
Read
Edit / Write
Run allowed commands
Add / update tests
Return changed files
Return command results
Return blockers
```

Executor 不拥有：

```text
修改任务目标的权力
扩大权限的权力
自行降低验收标准的权力
宣布最终验收通过的权力
```

---

## Why this matters for long-running agents

一次性代码生成主要考验模型能力。

连续几十轮的软件工程任务则开始考验：

- 状态；
- 边界；
- 责任；
- 验证；
- 恢复；
- 人机协作。

Task Contract 是 NovaWing 用来把这些工程问题固定下来的基础单位之一。
