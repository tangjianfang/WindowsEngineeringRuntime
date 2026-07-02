# Kubernetes

> Status: Draft
>
> Last Updated: 2026-07-02

---

Kubernetes 相关参考资料（架构借鉴）。

## 索引

| 主题 | 说明 | 链接 |
| ---- | ---- | ---- |
| Kubernetes 官方文档 | 容器编排系统总览 | https://kubernetes.io/docs/home/ |
| Controller / Reconciliation Loop | "期望状态 vs 实际状态"持续调谐模式 | https://kubernetes.io/docs/concepts/architecture/controller/ |
| CRD（Custom Resource Definition） | 可扩展资源模型，声明式 API 的核心机制 | https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/ |
| Operator 模式 | 将运维知识编码为控制器，自动化管理有状态应用生命周期 | https://kubernetes.io/docs/concepts/extend-kubernetes/operator/ |

## 与本项目的关联（架构借鉴，非直接依赖）

- **声明式目标 + 调谐循环**：与本项目 Goal First 原则高度相似——用户声明"目标状态"（如"支持新的 HID 指令"），
  Runtime（类比 Controller）持续调谐，驱动系统从当前状态趋近目标状态，可作为 `04_Runtime/Kernel` 调度机制的
  架构参照。
- **CRD 的可扩展资源模型**：与 Capability 的"稳定接口 + 可插拔底层实现"思路一致，是 `Capability` 抽象设计
  的可选参考范式。
- 需注意 Kubernetes 面向分布式集群资源编排，本项目面向单机 Windows Native 工程生命周期管理，
  借鉴的是架构模式而非具体技术栈。
