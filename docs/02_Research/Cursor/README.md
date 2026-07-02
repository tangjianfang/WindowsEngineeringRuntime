# Cursor

> Status: Draft
>
> Last Updated: 2026-07-02

---

Cursor 相关调研笔记。

## 1. 定位

基于 VS Code 二次开发的 AI 原生代码编辑器，将模型能力深度嵌入编辑器内（内联补全、Chat、Composer/Agent
模式），面向 IDE 内的交互式开发体验。

## 2. 核心机制

- **Codebase Indexing**：对整个仓库建立语义索引（embedding），支持"@codebase"式跨文件语义检索，
  而不仅依赖当前打开文件的上下文。
- **多档位交互**：Tab 补全（局部）、Chat（问答）、Composer/Agent（多文件自主编辑）三档能力，
  按任务复杂度递进使用工具调用与自主性。
- **Rules（.cursor/rules）**：项目级规则文件，约束模型在特定目录/语言下的行为（类似 Claude Code 的 CLAUDE.md）。
- **Checkpoints**：Agent 模式下对每一步文件改动做快照，支持一键回滚，降低自主编辑的风险。

## 3. 对本项目的借鉴点

- **语义化 Context 检索**：`04_Runtime/Context` 领域可参考其"仓库级语义索引"思路，作为工程上下文获取的
  一种实现手段。
- **Checkpoints / 可回滚性**：与 Verification First 原则一致，是 `Workflow` 设计中"可验证、可回滚"的
  具体参照案例。
- **项目级规则文件**：进一步印证"项目约定应显式沉淀为文件供 Agent 读取"这一通用模式。

## 4. 局限 / 与本项目的差异

- 仍以"编辑器内的通用代码生成/编辑"为核心场景，未对 Windows Native 外设通信、设备调试等细分工程能力
  做深度建模。
- Agent 模式与具体模型/Provider 强绑定，不满足 Provider Independent 原则。

## 5. 参考

- Cursor 官方文档：https://docs.cursor.com/
