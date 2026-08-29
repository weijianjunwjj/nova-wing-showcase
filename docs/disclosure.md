# Public Disclosure Boundary

NovaWing Development Guardian 的核心实现位于私有仓库。本 Showcase 的目标是证明项目真实存在、架构完整、工程方向清晰，同时避免为了求职展示而过度公开可能具有知识产权价值的实现细节。

## Public by design

以下内容适合公开：

- 产品定位与解决的问题
- 高层架构图
- Brain / Executor 职责划分
- Spec-driven development 的工作方法
- Task 01–99 的任务化思想
- Verification / Approval / Recovery 的高层流程
- 脱敏后的控制台截图
- 脱敏后的任务执行案例
- 不包含关键实现细节的技术文章
- 已经明确决定公开的通用组件或工具

## Private by default

以下内容默认保持私有：

- 核心源码
- 完整 System Prompt / Brain Prompt / Reviewer Prompt
- Executor 完整适配代码
- 内部状态持久化格式
- Checkpoint / Recovery 的关键算法与协议细节
- Approval / risk decision 的完整内部实现
- 用于限制或授权工具调用的内部策略
- 未公开的安全设计细节
- 私有测试夹具与内部攻击 / failure cases
- 后续可能用于知识产权或专利规划的技术方案细节

## Evidence without source disclosure

不公开源码并不等于无法验证项目。

本 Showcase 优先使用以下方式建立可信度：

1. **Architecture** — 公开系统角色、边界与数据流；
2. **Runtime screenshots** — 证明真实系统已经运行；
3. **Task lifecycle** — 展示从规格到验收的完整流程；
4. **Verification evidence** — 展示真实测试 / 构建 / 检查结果；
5. **Recovery evidence** — 展示中断后从可信进度继续；
6. **Technical discussion** — 在面试或合作沟通中解释设计取舍。

这样可以回答技术负责人最重要的问题：

> “这个系统是不是真的做出来了？”

同时不需要回答：

> “能不能把全部核心代码复制走？”

## IP note

公开仓库本身不代表对核心 NovaWing 实现授予任何开源许可。

对于可能涉及专利、新颖性判断、商业秘密或其他知识产权的问题，应在正式公开具体技术方案前结合实际申请地区和专业意见单独评估。本文件只定义项目当前的工程披露策略，不构成法律意见。
