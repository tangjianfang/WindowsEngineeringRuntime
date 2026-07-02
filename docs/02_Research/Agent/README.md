# Agent

> Status: Draft
>
> Last Updated: 2026-07-02

---

Agent 通用机制相关调研笔记。

## 1. 定位

跨具体产品（Claude Code / Cursor / Codex CLI 等）提炼的"Agent"通用机制调研，
关注让 LLM 在多轮迭代中自主使用工具、观察反馈并推进任务的通用模式，而非某一家产品的实现细节。

## 2. 通用核心机制

- **Agent Loop（感知-规划-行动-观察）**：模型接收当前状态与目标 → 规划下一步 → 调用工具执行 →
  将工具结果重新纳入上下文 → 循环直至任务完成或需要人工介入。这是所有主流 Coding Agent 的共同骨架。
- **工具调用（Tool Use / Function Calling）**：将副作用操作（文件读写、命令执行、网络请求）封装为
  结构化、可校验参数的"工具"，而非让模型直接输出自由文本去执行。
- **上下文管理**：由于上下文窗口有限，需要机制对历史信息做摘要/裁剪/检索（长任务的关键工程难点），
  对应本项目 `04_Runtime/Context` 与 `Memory` 领域。
- **子任务委派（Subagents / Delegation）**：将独立、可并行、结果可摘要的子任务派发给独立上下文的
  子 Agent，主上下文只保留精简结论，降低主任务的上下文膨胀。
- **风险分级与人工确认（Human-in-the-loop）**：对不可逆/高风险操作（写文件、执行命令、推送代码）
  设置审批/沙箱/回滚机制。

## 3. 对本项目的借鉴点

- Agent Loop 与工具调用的骨架，是 `04_Runtime/Agent` 领域设计的通用理论基础。
- 上下文管理与子任务委派机制，直接对应 `Context`、`Memory` 两个领域，未来设计时应参考。
- 风险分级确认机制，与 Verification First 原则一致，是 `Workflow` 设计中"任务如何安全推进"的关键输入。

## 4. 局限 / 与本项目的差异

- 上述机制均是"通用 Agent 骨架"，尚未涉及 Windows Native 工程特有的验证方式（如设备连接状态、
  编译产物签名校验、驱动加载等），这部分仍需本项目结合 Capability 设计补充。

## 5. 参考

- Anthropic「Building Effective Agents」：https://www.anthropic.com/research/building-effective-agents
- ReAct: Synergizing Reasoning and Acting in Language Models（Yao et al., 2022）：https://arxiv.org/abs/2210.03629
