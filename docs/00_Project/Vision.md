# Vision

> Status: Draft
>
> Last Updated: 2026-07-02
>
> Owners: @tangjianfang

---

## 愿景

> 将 Windows Native 软件工程从 Tool Driven 转变为 Goal Driven。

## 未来图景

工程师向 Runtime 描述**目标**，例如"支持新的 HID 指令"，Runtime 负责：

1. 理解目标，拆解为可执行的工程 Capability。
2. 调度底层 Tool 完成 Build / Test / Benchmark 等步骤。
3. 通过可验证的结果确认目标达成。
4. 全过程可追溯、可复现，不依赖对 AI 的信任。

## 长期方向

- 稳定的 Capability 抽象层。
- 可插拔的 Provider 与 Plugin 生态。
- 面向 Windows Native 的参考实现（C++20）。
