# MCP

> Status: Draft
>
> Last Updated: 2026-07-02

---

Model Context Protocol (MCP) 相关调研笔记。

## 1. 定位

Anthropic 提出并开源的开放协议，用于统一"AI 应用（Host/Client）"与"外部数据源/工具（Server）"之间的
交互方式，解决"每个 AI 应用都要为每个工具单独写一套集成"的 M×N 集成问题。

## 2. 核心机制

- **三种角色**：Host（承载模型的应用，如 Claude Desktop/IDE 插件）、Client（Host 内与单个 Server 建立
  1:1 连接的组件）、Server（暴露能力的独立进程/服务）。
- **三类原语（Primitives）**：
  - `Tools` — 可被模型调用执行动作的函数（有副作用，由模型决定是否调用）。
  - `Resources` — 只读上下文数据（由应用决定何时加载，类似"文件/数据引用"）。
  - `Prompts` — 预定义的提示模板，供用户显式触发。
- **传输层**：本地场景用 stdio，远程场景用 HTTP + Server-Sent Events（或可流式 HTTP），消息体使用
  JSON-RPC 2.0。
- **能力协商（Capability Negotiation）**：Client/Server 建连时交换各自支持的能力集合，按需启用。

## 3. 对本项目的借鉴点

- **Tool / Resource / Prompt 三分**与本项目 `Capability`（能力）、`Context`（上下文）等模块的划分思路
  高度一致，可作为 `04_Runtime/Capability` 与 `Provider` 接口设计的直接参考协议。
- **Provider Independent**：MCP 本身与具体模型厂商无关，是"标准化工具接入协议"的成熟范例，
  未来若 Runtime 需要对接外部工具生态（如 USB 抓包工具、调试器），可优先评估是否复用/兼容 MCP。
- **能力协商机制**：为 `Capability` 的"底层 Tool 可替换"提供了具体的协议级实现范式。

## 4. 局限 / 与本项目的差异

- MCP 关注"AI 应用与工具间的连接协议"，不涉及工程目标（Goal）到 Capability 的拆解与调度，
  这部分仍需 Runtime 自身设计（对应 `04_Runtime/Kernel` 与 `Workflow`）。

## 5. 参考

- MCP 官方规范：https://modelcontextprotocol.io/
- MCP GitHub 组织：https://github.com/modelcontextprotocol
