# _templates

文档模板目录。

新建任何标准文档时，请从此处复制对应模板，并按命名规范重命名：

| 模板 | 用途 | 目标目录 | 命名 |
|------|------|----------|------|
| `PS_Template.md` | Problem Statement | `01_Problem/` | `PS-0001.md` |
| `RFC_Template.md` | 方案提案 | `03_Architecture/RFC/` | `RFC-0001.md` |
| `ADR_Template.md` | 架构决策记录 | `03_Architecture/ADR/` | `ADR-0001.md` |
| `EXP_Template.md` | Prototype 实验记录 | `03_Architecture/Prototype/` | `EXP-0001.md` |

所有文档头部必须包含 `Status` 字段，状态流转见根 `README.md` 第 6.1 节。
交叉引用规范见根 `README.md` 第 6.2 节。
