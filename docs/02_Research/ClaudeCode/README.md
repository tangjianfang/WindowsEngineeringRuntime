# ClaudeCode

> Status: Draft
>
> Last Updated: 2026-07-02

---

Claude Code 相关调研笔记。

## 1. 定位

Anthropic 官方发布的终端原生 Agentic Coding 工具，运行在开发者本地终端/CI 中，直接读写文件系统、执行 Shell
命令、调用 Git，面向"在真实代码库中完成端到端工程任务"，而非聊天问答。

## 2. 核心机制

- **Agent Loop**：读取上下文 → 规划 → 调用工具（文件读写 / Bash / 搜索 / 编辑）→ 观察结果 → 继续迭代，直至任务
  完成或需要用户确认。
- **工具化（Tool Use）**：将文件编辑、命令执行、搜索等能力封装为结构化工具调用，而不是让模型直接生成整份文件。
- **Plan Mode / 分步确认**：对高风险操作（如批量修改、执行命令）支持先出计划、后执行，降低破坏性。
- **CLAUDE.md 项目记忆**：项目根目录可放置 `CLAUDE.md` 记录仓库约定、常用命令、注意事项，供每次会话读取，
  降低重复解释成本。
- **子代理（Subagents）**：可将耗时的探索/验证类任务派发给独立上下文的子代理执行，主上下文只接收摘要结果。

## 3. 对本项目的借鉴点

- **Tool 抽象 vs 直接生成**：与本项目 Capability First 原则一致——上层只与稳定的"能力接口"交互，具体执行细节
  下沉到工具实现，便于替换底层实现而不影响调用方。
- **项目级记忆文件**：`CLAUDE.md` 的思路与本仓库 `04_Runtime/Memory` 领域的目标（记忆与状态管理）相关，
  可作为该领域设计时的参考输入。
- **计划优先、可中断确认**：与 Verification First（可验证结果）原则相呼应，值得在 `Workflow` 设计中借鉴。

## 4. 局限 / 与本项目的差异

- Claude Code 面向通用软件工程，对 Windows Native 细分领域（USB/HID/DXGI 等外设通信）没有针对性支持，
  这正是 `PS-0001` 所指出的缺口。
- 其运行时状态与具体 Provider（Claude 模型）强绑定，不满足本项目 Provider Independent 原则。

## 5. 参考

- Anthropic 官方文档：https://docs.anthropic.com/en/docs/claude-code/overview
- Anthropic Engineering Blog（Claude Code 最佳实践）：https://www.anthropic.com/engineering/claude-code-best-practices
