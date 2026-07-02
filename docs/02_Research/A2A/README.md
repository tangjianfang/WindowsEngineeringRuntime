# A2A

> Status: Draft
>
> Last Updated: 2026-07-02

---

Agent-to-Agent (A2A) 相关调研笔记。

## 1. 定位

由 Google 发起、现由 Linux Foundation 托管的开放协议，用于让**不同厂商、不同框架构建的 Agent**之间
互相发现能力、协商交互方式并协作完成任务，解决"多 Agent 系统之间缺乏统一通信标准"的问题。与 MCP
（Agent 对接工具/数据）互补——A2A 面向 Agent 对接 Agent。

## 2. 核心机制

- **Agent Card**：每个 Agent 暴露一份 JSON 格式的"名片"，描述其身份、能力（Skills）、输入输出模态、
  认证方式，供其他 Agent 发现与匹配。
- **Task 生命周期**：交互以 Task 为单位，具有明确状态机（如 submitted / working / input-required /
  completed / failed），支持长时任务与中途需要人工/上游输入的场景。
- **消息与产物（Artifacts）**：Agent 间以结构化 Message 交换信息，任务完成后可产出 Artifact（文件、
  结构化数据等）。
- **传输**：基于 HTTP，支持同步请求/响应、Server-Sent Events 流式更新、以及推送通知（Webhook）三种交互模式。

## 3. 对本项目的借鉴点

- **Agent Card 的能力声明方式**，可作为 `04_Runtime/Agent` 设计"Agent 能力发现与注册"机制时的参考范式，
  尤其是未来若 Runtime 需要编排多个专职 Agent（如 Planner / Builder / Debugger）协作时。
- **Task 状态机**与本项目 Verification First 原则一致（工程结果需可追踪、可验证到终态），
  可作为 `Workflow` 领域任务状态建模的参照。

## 4. 局限 / 与本项目的差异

- A2A 解决的是"跨组织、跨框架 Agent 互操作"问题，本项目当前阶段的多 Agent 协作大概率发生在同一
  Runtime 进程内，暂不需要跨网络的 Agent Card 发现机制，但其状态机与能力声明的设计理念仍有参考价值。

## 5. 参考

- A2A 官方规范（Linux Foundation）：https://a2a-protocol.org/
- A2A GitHub 组织：https://github.com/a2aproject
