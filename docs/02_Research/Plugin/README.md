# Plugin

> Status: Draft
>
> Last Updated: 2026-07-02

---

Plugin（插件）机制相关调研笔记。

## 1. 定位

"宿主应用定义稳定的扩展点（Extension Point）与清单格式（Manifest），第三方在不修改宿主核心代码的前提下
新增功能"这一通用软件工程模式。在当前 AI 工具生态中常见形态包括：IDE 插件（VS Code Extension API）、
Claude Code 近期推出的 Plugin/Marketplace 机制（将 Slash Command、Subagent、MCP Server、Hook 打包为可安装
单元发布）等。本质是**扩展与分发机制**，回答"第三方如何新增能力并被宿主发现、安装、启用"的问题。

## 2. 核心机制

- **扩展点 + 清单文件（Manifest）**：插件在清单中声明自己扩展哪些点位、依赖哪些权限/能力。
- **生命周期钩子**：安装/激活/停用/卸载等生命周期事件，供插件注册回调。
- **分发与发现（Marketplace/Registry）**：统一的发布、检索、版本管理渠道，降低第三方扩展的分发成本。
- **与 MCP 的关系**：新一代 AI 工具的 Plugin 机制常常将 MCP Server 作为其中一类可被打包的扩展能力——
  Plugin 是更上层的"打包/生命周期/分发"机制，MCP 是其中承载"工具调用"的协议之一，两者不是同一层面的
  竞争关系。

## 3. 对本项目的借鉴点

- `05_SDK/PluginSDK.md` 的既有定位（第三方为 `Capability` 提供新的底层 Tool 实现）与此模式一致，未来展开
  设计时可参考"清单声明扩展点 + 权限声明"的通用结构。
- Marketplace/Registry 的分发与发现思路，对未来 `Capability`/Skill 如何被登记、发现、版本管理（呼应
  `ADR-0001` 提出的"索引优先于目录"）有参考价值。

## 4. 局限 / 与本项目的差异

- Plugin 是纯粹的扩展与分发机制，不解决工程 Goal 如何被拆解、执行（那是 `Agent`/`Workflow` 的职责），
  也不解决跨 Provider 的协议标准化问题（那是 MCP 的职责）。
- 不应把 Plugin 当作本项目对外交互的主范式，而应定位为 `Capability`/Skill 的第三方扩展与分发通道，
  即"生态扩展"问题，不是"AI 交互范式"问题。

## 5. 参考

- VS Code Extension API：https://code.visualstudio.com/api
- Claude Code Plugin 相关说明：https://docs.claude.com/
