# Demo & Evidence

本页用于集中展示 NovaWing Development Guardian 的真实运行证据。

> 当前公开仓库不包含核心源码。后续可持续补充脱敏截图、录屏和任务案例，用来证明系统确实在真实研发流程中运行，而不是概念图或 Prompt Demo。

## Recommended evidence

建议优先补充以下内容，价值从高到低排列：

### 1. Full task lifecycle

展示一个完整 Task 从开始到完成的过程：

```text
Task selected
→ Brain reviewing
→ Executor running
→ Verification
→ Brain review
→ Completed
```

最好使用同一个任务的连续截图，避免只展示零散 UI。

### 2. Brain / Executor separation

展示同一轮中的两个视角：

- Brain / Reviewer 给出的 delegation / decision；
- Executor 实际完成的代码、测试与结果摘要。

目的不是公开完整 Prompt，而是让读者一眼看懂：

> **GPT 在做判断，Claude Code / Codex 在做执行。**

### 3. Verification evidence

适合公开：

- test passed / failed
- typecheck
- lint
- build
- deterministic repository checks

不建议只放“AI says done”。

### 4. Human approval

展示系统遇到关键决策时进入 suspended / human-required 状态，以及人工确认后继续运行的过程。

需要隐藏：

- 完整内部审批协议；
- 敏感命令；
- 环境信息；
- token / secret / private path。

### 5. Recovery

这是 NovaWing 很有辨识度的一类证据。

推荐用三张图说明：

1. 长任务中断；
2. 系统识别可恢复 checkpoint；
3. Executor 从已有进度继续，而不是从零重做。

### 6. Control UI

适合展示的信息：

- 当前 phase / status；
- 当前 task；
- iteration；
- recent activity；
- verification 状态；
- approval / recovery 状态。

---

## Suggested public asset structure

后续有截图后，可以按下面的路径上传：

```text
assets/
├─ overview/
│  └─ guardian-control-ui.png
├─ workflow/
│  ├─ 01-brain-review.png
│  ├─ 02-executor-running.png
│  ├─ 03-verification.png
│  └─ 04-completed.png
├─ approval/
│  ├─ suspended.png
│  └─ resumed.png
└─ recovery/
   ├─ interrupted.png
   ├─ checkpoint.png
   └─ recovered.png
```

然后在本页追加：

```md
![Guardian Control UI](../assets/overview/guardian-control-ui.png)
```

---

## Demo video

录屏比源码更适合当前阶段的公开展示，因为它可以证明系统真实运行，同时控制知识产权披露范围。

推荐录制一个 2–5 分钟版本：

```text
00:00  介绍 Task / Acceptance Criteria
00:20  GPT Brain 判断下一步
00:45  Claude Code / Codex 执行
01:30  自动 Verification
02:00  Brain Review
02:30  完成 / 审批 / Recovery
```

如果有 10 分钟以上的完整运行录屏，可以保留为 long-form evidence；对招聘方则额外制作一个更短的核心流程版本。

---

## Sanitization checklist

公开任何截图、日志或视频前检查：

- [ ] 没有 API Key / Token / Cookie
- [ ] 没有本机用户名、绝对路径等隐私信息
- [ ] 没有私有仓库 URL 或内部 package token
- [ ] 没有完整 System Prompt / Reviewer Prompt
- [ ] 没有尚未公开的核心算法或协议细节
- [ ] 没有真实业务数据
- [ ] 没有第三方受版权 / 保密约束的代码
- [ ] 没有可能影响后续知识产权规划的关键实现细节

---

## What counts as convincing evidence?

对技术负责人而言，最有说服力的通常不是代码行数，而是：

> **一个真实复杂 Task 被规格化、执行、验证，在遇到问题后还能被审查、暂停或恢复，最终完成验收。**

这也是本 Showcase 后续最值得持续补充的内容。
