# Skill

> Status: Draft
>
> Last Updated: 2026-07-02

---

Skill（技能包）相关调研笔记。

## 1. 定位

以 Anthropic Agent Skills（Claude Skills）为代表的"可发现、按需加载的领域知识与流程封装"机制：将某一细分领域的
说明文档、操作步骤、可选脚本/资源打包成一个独立单元（如 `SKILL.md` + 附属文件），供 Agent 在需要时自主发现并
加载。本质是**知识/流程的封装与分发格式**，而不是新的执行引擎或协议——加载后仍依赖宿主 Agent 的工具调用能力
去实际执行。可泛化理解为不限于某一家产品形态的通用"领域知识包"范式。

## 2. 核心机制

- **声明式元数据**：技能包含简短的 name/description（用途、适用场景），Agent 可以低成本地"浏览目录"决定是否
  需要加载，而不必一次性读入全部内容。
- **渐进式加载（Progressive Disclosure）**：先加载摘要信息，只有确认相关时才展开完整正文、引用文件或脚本，
  避免一次性占满上下文窗口。
- **可携带资源/脚本**：技能包目录下可以附带参考资料、模板、可执行脚本，Agent 按需调用这些资源，而不是要求
  模型凭空生成同等内容。
- **与工具调用的关系**：技能包提供"怎么做"的结构化知识，真正的动作（读写文件、调用命令、访问设备）仍通过
  Agent 已有的工具调用/能力体系完成，两者是配合关系而非替代关系。

## 3. 对本项目的借鉴点

- `PS-0001` 指出的核心痛点——Windows Native 细分领域（USB/HID 等外设通信、音视频、GUI）缺乏深度模块化——
  正是 Skill 机制最适合解决的问题：可以把 `06_Reference/USB/README.md` 等协议知识结构化封装为独立技能包，
  供任意 Agent（无论是本项目未来的内部 Runtime Agent，还是外部 Claude Code/Cursor 等工具）按需加载复用，
  不因更换 AI Provider 或工具而失效。
- 渐进式加载思路对 `04_Runtime/Context`、`Memory` 领域"上下文/知识如何按需注入、避免膨胀"的设计有直接参考
  价值。
- Skill 与 `Capability` 是互补关系而非重叠关系：Skill 是声明式、可读的领域知识，`Capability` 是可执行、
  可验证的工程能力；同一个细分领域（如 USB）未来可以同时拥有一个 Skill（知识层）与一个 Capability（执行层）。

## 4. 局限 / 与本项目的差异

- Skill 本身不执行任何动作，也不产出可验证的结果，无法替代 `Capability`/`Verification`——只能提供指导，
  不能满足 Verification First 原则，必须与 `Capability`/`Workflow` 结合才能形成闭环。
- 目前主流 Skill 生态（如 Claude 官方 Agent Skills）与特定 Provider 绑定较深（格式、加载机制由该 Provider
  的宿主环境定义），若要满足 Provider Independent 原则，本项目应将"Skill"当作通用的知识封装范式来设计
  （可被任意 Agent/Provider 消费的结构），而非直接照搬某一家的私有实现细节。

## 5. 参考

- Anthropic 官方发布与文档站点（Agent Skills / Claude Skills 说明）：https://www.anthropic.com/ 、
  https://docs.claude.com/
