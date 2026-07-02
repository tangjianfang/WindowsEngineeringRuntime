# Windows Engineering Runtime Framework

> Project Guide v0.1
>
> Last Updated: 2026-07-02
>
> Status: Draft
---

# 1. 项目目标

本项目不是为了实现另一个 AI Chat、Claude Code 或 Cursor。

本项目旨在探索并构建一套 **Windows Engineering Runtime Framework**。

Runtime 的职责不是生成代码，而是管理整个工程生命周期（Engineering Lifecycle）。

最终目标：

> 将 Windows Native 软件工程从 **Tool Driven** 转变为 **Goal Driven**。

---

# 2. 项目原则

所有设计必须遵循以下原则。

## Principle 1

Goal First

Runtime 接收的是工程目标（Goal），而不是 Prompt。

例如：

```
支持新的 HID 指令
```

而不是：

```
帮我写代码
```

---

## Principle 2

Engineering First

工程能力高于代码生成。

代码只是工程过程中的一种结果。

---

## Principle 3

Capability First

Runtime 管理 Capability，而不是 Tool。

例如：

```
Capability
    Build Project
```

底层可以调用：

```
MSBuild

Ninja

CMake
```

Tool 可以变化。

Capability 保持稳定。

---

## Principle 4

Verification First

所有工程结果必须可验证。

不能相信 AI。

只能相信：

Build

Test

Benchmark

Device

Verification

---

## Principle 5

Provider Independent

Runtime 不依赖任何 AI Provider。

支持：

Claude

GPT

Gemini

Qwen

DeepSeek

Local Model

---

# 3. 文档体系

```
WindowsEngineeringRuntime/

│
├── README.md
│
├── docs/
│
│   ├── _templates/
│   │
│   │   PS_Template.md
│   │   RFC_Template.md
│   │   ADR_Template.md
│   │   EXP_Template.md
│   │
│   ├── 00_Project/
│   │
│   │   Project_Charter.md
│   │   Vision.md
│   │   Roadmap.md
│   │
│   ├── 01_Problem/
│   │
│   │   PS-0001.md
│   │   UserStories.md
│   │   PainPoints.md
│   │   DailyWorkflow.md
│   │
│   ├── 02_Research/
│   │
│   │   ClaudeCode/
│   │   Cursor/
│   │   CodexCLI/
│   │   MCP/
│   │   A2A/
│   │   Agent/
│   │
│   ├── 03_Architecture/
│   │
│   │   ADR/
│   │   RFC/
│   │   Prototype/
│   │
│   ├── 04_Runtime/
│   │
│   │   Kernel/
│   │   Context/
│   │   Capability/
│   │   Workflow/
│   │   Agent/
│   │   Memory/
│   │   EventBus/
│   │   Provider/
│   │
│   ├── 05_SDK/
│   │
│   │   CapabilitySDK.md
│   │   ProviderSDK.md
│   │   WorkflowSDK.md
│   │   PluginSDK.md
│   │
│   ├── 06_Reference/
│   │
│   │   Papers/
│   │   Books/
│   │   Windows/
│   │   LLVM/
│   │   Kubernetes/
│   │
│   └── 99_Archive/
│
└── runtime/
```

---

# 4. 每个目录的职责

## 00_Project

项目级文档。

回答：

为什么做？

未来想做到什么？

路线图是什么？

---

## 01_Problem

只讨论问题。

禁止讨论实现。

禁止讨论代码。

回答：

用户是谁？

痛点是什么？

今天如何工作？

未来希望怎样工作？

---

## 02_Research

所有调研资料。

例如：

Claude Code

Cursor

MCP

A2A

Agent

Plugin

Workflow

Skill

所有阅读笔记放这里。

---

## 03_Architecture

真正的架构设计。

包括：

RFC

ADR

Prototype

这里才讨论：

Kernel

Capability

Workflow

Context

---

## 04_Runtime

真正 Runtime 的设计。

这里只讨论：

Runtime 自己。

不讨论：

具体业务。

---

## 05_SDK

未来开放接口。

例如：

Capability SDK

Provider SDK

Workflow SDK

Plugin SDK

---

## 06_Reference

参考资料。

例如：

Windows Internals

Design Pattern

Papers

Books

官方文档

---

## 99_Archive

历史版本。

已经废弃的设计。

不要删除。

只归档。

---

# 5. 文档类型

本项目统一采用五种核心文档（Project Charter / Problem Statement / RFC / ADR / Prototype）。

此外，`02_Research` 下的 Research 笔记为辅助文档，不计入五种核心文档，命名规范见第 6 节。

---

## Project Charter

回答：

项目为什么存在。

什么时候算成功。

---

## Problem Statement（PS）

回答：

真正的问题是什么。

任何架构都必须引用 Problem。

---

## RFC

提出一种方案。

允许被推翻。

允许讨论。

允许修改。

---

## ADR

Architecture Decision Record

表示：

已经达成共识。

已经经过讨论。

已经验证。

原则上不能随意修改。

---

## Prototype

实验记录。

记录：

实验目的。

实验结果。

优点。

缺点。

下一步。

---

# 6. 文档命名规范

Project

```
Project_Charter.md
```

Problem

```
PS-0001.md
```

RFC

```
RFC-0001.md
```

ADR

```
ADR-0001.md
```

Prototype

```
EXP-0001.md
```

Research

```
ClaudeCode_Runtime.md

Cursor_Architecture.md

MCP_Research.md
```

---

# 6.1 文档状态机

每一份文档头部必须标注 `Status` 字段，取值范围如下：

```
Draft        草稿，正在编写
Review       评审中
Accepted     已达成共识（ADR 常用）
Superseded   已被新文档取代
Archived     已归档（移入 99_Archive）
```

状态流转：

```
Draft → Review → Accepted → Superseded/Archived
```

任何文档不得跳过 Review 直接进入 Accepted。

---

# 6.2 交叉引用规范

为保证"每一个架构决策都必须可追溯"，规定：

1. 每一个 RFC / ADR 头部必须标注 `Relates-To: PS-XXXX`，引用其解决的 Problem。
2. 每一个 ADR 若由某个 RFC 升级而来，必须标注 `Derived-From: RFC-XXXX`。
3. 每一个 Prototype（EXP）必须标注其验证的 `RFC-XXXX` 或 `ADR-XXXX`。
4. 被取代的文档必须标注 `Superseded-By: <新文档编号>`。

---

# 7. 每次讨论后的工作流程

每次完成讨论后：

Step 1

记录讨论内容。

放入：

```
99_Archive/
Discussion/
```

Step 2

提炼真正的问题。

如果属于：

Problem

更新：

PS。

如果属于：

Architecture

写：

RFC。

如果已经确认：

更新：

ADR。

Step 3

如果存在争议：

建立：

Prototype。

验证。

Step 4

Prototype 完成以后。

决定：

是否升级成 ADR。

---

# 8. 当前阶段路线图

Phase 0

项目立项

完成：

* Project Charter
* Vision
* Roadmap

---

Phase 1

Problem Definition

完成：

* PS-0001
* User Story
* Pain Points
* Daily Workflow

---

Phase 2

Research

完成：

Claude Code

Cursor

Codex CLI

Gemini CLI

Copilot

MCP

Plugin

Workflow

Agent

Skill

全部调研。

---

Phase 3

Architecture

开始：

Kernel

Capability

Context

Workflow

Agent

Event Bus

Memory

Provider

---

Phase 4

Prototype

最小 Runtime。

最小 Capability。

最小 Workflow。

---

Phase 5

Reference Implementation

使用 C++20 实现。

Windows Native。

MVP。

---

# 9. 项目纪律

所有设计必须遵守以下规则：

1. 不先写代码，再补架构。
2. 不先设计模块，再定义问题。
3. 不为了 AI 而设计 Runtime。
4. Runtime 必须独立于任何 LLM。
5. 每一个新增模块都必须说明存在理由。
6. 每一个 Capability 都必须可以验证。
7. 每一个架构决策都必须可追溯。
8. 每一个设计都允许被 Prototype 推翻。

---

# 10. 当前阶段目标

目前阶段：

> 不实现 Runtime。

只完成：

Project Charter

Problem Statement

Research

Architecture Draft

建立完整知识体系。

只有当问题足够清晰、架构足够稳定之后，才进入代码实现阶段。

---

# Motto

> Build an Engineering Runtime,
>
> not another AI Coding Tool.
