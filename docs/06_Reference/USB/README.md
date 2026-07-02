# USB

> Status: Draft
>
> Last Updated: 2026-07-02
>
> Owners: @tangjianfang

---

USB 协议相关规范集合。服务于"外设通信协议抓包分析"这一目标（Relates-To: `01_Problem/PS-0001.md`）。

本目录**只做规范摘要与索引**，不转载 USB-IF / 各 Class 规范的受版权保护原文；每篇文档给出背景、适用范围、
与抓包分析相关的关键结构/字段，以及官方规范的获取链接。

## 1. 为什么整理这些规范

`PS-0001` 指出，当前主流 AI 开发工具对 Windows Native 工程中"外设交互通信（USB/HID/Device）"缺乏深度
模块化支持。要具备"协议抓包分析"能力，前提是先系统性掌握 USB 协议栈各层规范，形成统一的知识底座，
未来可作为 `04_Runtime/Capability` 下 USB 子领域（Capability/USB/）设计与实现的输入。

## 2. 索引

| 文档 | 层级 | 说明 | Status |
| ---- | ---- | ---- | ------ |
| [USB_Core_Spec.md](USB_Core_Spec.md) | 核心协议（USB 2.0 / USB 3.x） | 总线拓扑、传输类型、标准设备请求、描述符体系 | Draft |
| [HID.md](HID.md) | Class 协议 | 人机接口设备（键盘/鼠标/游戏杆/自定义 HID）报告描述符与传输模型 | Draft |
| [MassStorageClass.md](MassStorageClass.md) | Class 协议 | 大容量存储（U盘等）Bulk-Only Transport，CBW/CSW | Draft |
| [AudioVideoClass.md](AudioVideoClass.md) | Class 协议 | 音频类（UAC）/ 视频类（UVC）描述符与流协议 | Draft |
| [CDC.md](CDC.md) | Class 协议 | 通信设备类（虚拟串口等），常见于调试器/传感器外设 | Draft |
| [CaptureAnalysis.md](CaptureAnalysis.md) | 工具与方法 | Windows 下 USB 抓包工具链与协议解析要点 | Draft |

## 3. 与仓库其他部分的关系

- `Relates-To: PS-0001`（`01_Problem/PS-0001.md`）——本集合是该 Problem Statement 所列"外设交互通信"痛点
  的知识准备工作。
- 未来若该领域内容积累到"至少三篇文档确定要放入"且需要沉淀为正式 Capability 设计，将按 ADR-0001
  的生长式目录原则，在 `04_Runtime/Capability/USB/` 建立完整的 ADR / RFC / Prototype / SDK 结构，
  届时本目录内容作为其背景资料输入，不直接等同于该设计本身。
- 与 `06_Reference/Windows/README.md` 中列出的 USB 驱动开发（WDK/UMDF）、ETW 抓包相关链接互为补充。

## 4. 版权与引用说明

- USB-IF 官方规范（usb.org）、各 Class 规范原文受版权保护，本目录不转载全文，仅总结公开可查的结构性
  知识点并附官方链接，供后续查阅原文使用。
