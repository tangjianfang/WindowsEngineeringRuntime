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

## 目录组织原则（见 ADR-0001）

`04_Runtime` 采用**生长式目录（Growing Tree）**，不预先按远期目标形态一次性展开：

1. **分阶段生长**：先只有少数一级目录（现状即阶段一）；某个概念（如 `Capability`）内容积累到一定规模后，
   才拆分为子领域目录（如 `Build/ Git/ USB/ DXGI/`）；子领域继续成长后，再为其建立 ADR / RFC / Prototype /
   SDK / Examples 等完整结构。
2. **三篇文档门槛**：任何新建子目录，必须已经有至少三篇文档确定要放入，否则不允许创建，避免出现空目录。
3. **索引优先于目录**：每个模块的 `README.md` 应维护一张索引表登记其内部条目（例如 Capability 列表），
   而不是提前建空目录去承载信息，参考格式：

   | 名称 | Status | ADR | SDK | Prototype |
   | ---- | ------ | --- | --- | --------- |

远期可能演进为按 DDD（Domain Driven Design）组织的更大目录（Kernel / Core / Capability / Workflow / Agent /
Context / Provider / SDK / Common / RuntimeHost / Verification / Scheduler / EventBus），该形态仅作为方向参考，
记录于 `99_Archive/Discussion/2026-07-02_Runtime目录组织方式.md`，不是当前必须实现的目录结构。
