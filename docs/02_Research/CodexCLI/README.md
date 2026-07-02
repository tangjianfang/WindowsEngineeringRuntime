# CodexCLI

> Status: Draft
>
> Last Updated: 2026-07-02

---

Codex CLI 相关调研笔记。

## 1. 定位

OpenAI 官方发布的开源终端 Agentic Coding 工具，运行于本地终端或 CI 环境，直接读写文件系统并执行命令，
定位与 Claude Code 相似——面向终端内的端到端工程任务自动化。

## 2. 核心机制

- **沙箱与审批模式（Approval Modes）**：支持 `suggest` / `auto-edit` / `full-auto` 等不同自主程度的执行策略，
  在文件系统与网络访问上可配置沙箱隔离级别，兼顾自主性与安全性。
- **AGENTS.md 项目说明文件**：类似 CLAUDE.md，用于记录项目结构、构建/测试命令、代码约定，供模型读取。
- **可脚本化 / 非交互模式**：支持以非交互方式在 CI 流水线中调用，便于集成到自动化工程流程。

## 3. 对本项目的借鉴点

- **分级自主性（审批模式）**：为 `04_Runtime/Kernel`（调度与生命周期管理）提供了"按风险分级授权执行"的
  具体参照，可用于设计 Capability 执行前的确认/回滚策略。
- **CI 可脚本化调用**：与本项目"工程结果需可通过 Build/Test/Benchmark 验证"的 Verification First 原则一致，
  值得在 `Workflow` 与未来 CI 集成设计中参考。

## 4. 局限 / 与本项目的差异

- 同样面向通用代码任务，不针对 Windows Native 外设通信等细分领域。
- 与 OpenAI 模型生态强绑定，不满足 Provider Independent 原则。

## 5. 参考

- OpenAI Codex CLI GitHub 仓库：https://github.com/openai/codex
