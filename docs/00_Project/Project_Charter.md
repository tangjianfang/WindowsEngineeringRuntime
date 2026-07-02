# Project Charter

> Status: Draft
>
> Last Updated: 2026-07-02
>
> Owners: @tangjianfang

---

## 1. 项目为什么存在

Windows Native 软件工程长期处于 **Tool Driven（工具驱动）** 状态：工程师围绕 MSBuild、CMake、调试器、设备工具等一系列离散工具展开工作，工具的变化不断侵蚀工程流程的稳定性。

本项目旨在构建一套 **Windows Engineering Runtime Framework**，其职责不是生成代码，而是**管理整个工程生命周期（Engineering Lifecycle）**，从而将工程模式从 **Tool Driven** 转变为 **Goal Driven（目标驱动）**。

## 2. 项目边界

本项目**不是**：

- 另一个 AI Chat
- 另一个 Claude Code / Cursor
- 一个代码生成器

本项目**是**：

- 一个管理工程目标、能力（Capability）与验证（Verification）的运行时框架
- 独立于任何 AI Provider 的工程运行时

## 3. 什么时候算成功

- Runtime 能接收工程 **Goal**（而非 Prompt），并驱动其到可验证的结果。
- Capability 抽象稳定，底层 Tool 可替换而不影响上层。
- 所有工程结果均可通过 Build / Test / Benchmark / Device 验证。
- Runtime 可在不修改核心的前提下切换任意 AI Provider。

## 4. 核心原则

详见根 `README.md` 第 2 节（Goal First / Engineering First / Capability First / Verification First / Provider Independent）。
