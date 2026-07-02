# LLVM

> Status: Draft
>
> Last Updated: 2026-07-02

---

LLVM 相关参考资料。

## 索引

| 主题 | 说明 | 链接 |
| ---- | ---- | ---- |
| LLVM 官方文档 | 编译器基础设施总览 | https://llvm.org/docs/ |
| Clang | C/C++/Objective-C 前端 | https://clang.llvm.org/docs/ |
| LLD | LLVM 链接器 | https://lld.llvm.org/ |
| clangd / clang-tidy | 语言服务器与静态分析，可用于 Capability（Build/Lint）能力设计 | https://clangd.llvm.org/ 、 https://clang.llvm.org/extra/clang-tidy/ |
| LLVM IR | 中间表示，用于理解编译产物结构（诊断/调试相关能力设计参考） | https://llvm.org/docs/LangRef.html |

## 与本项目的关联

- `04_Runtime/Capability` 未来的 Build/Compiler 子领域可能封装 Clang/LLVM 工具链作为底层 Tool 之一；
  本目录用于沉淀相关调研，不涉及具体实现。
