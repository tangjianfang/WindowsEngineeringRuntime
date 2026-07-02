# 04_Runtime

真正 Runtime 的设计。这里只讨论 Runtime 自己，不讨论具体业务。

## 模块

- `Kernel/` — 运行时内核，调度与生命周期管理
- `Context/` — 工程上下文
- `Capability/` — 能力抽象（稳定），屏蔽底层 Tool
- `Workflow/` — 工作流编排
- `Agent/` — Agent 机制
- `Memory/` — 记忆与状态
- `EventBus/` — 事件总线
- `Provider/` — AI Provider 抽象（Provider Independent）

> 现阶段只做设计草稿，不实现 Runtime。
