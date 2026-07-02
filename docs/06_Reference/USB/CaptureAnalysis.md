# USB 抓包分析：工具与方法

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 目标

汇总在 Windows 平台对 USB 外设通信进行抓包与协议分析的常用工具链与方法论，作为
`04_Runtime/Capability` 未来 USB 子领域设计（协议解析器、诊断能力）的输入信息。

## 2. 常用抓包工具（Windows 平台）

| 工具 | 层级/方式 | 说明 |
| ---- | -------- | ---- |
| Wireshark + USBPcap | 软件抓包 | Windows 上最常用的免费方案，可抓取 Control/Bulk/Interrupt/Isochronous 各类传输，按 Wireshark 内建的 USB 解析规则展示描述符与请求 |
| Microsoft Message Analyzer（已停止维护） | 软件抓包 | 历史上微软官方工具，现推荐用 Wireshark/USBPcap 替代 |
| USBlyzer / USB Sniffer 类商业工具 | 软件抓包 | 提供更友好的描述符/枚举可视化，部分支持自动识别 Class 协议 |
| ETW（Event Tracing for Windows）USB Provider | 系统级追踪 | 结合 `usbhub`/`usbport` 等内建 ETW Provider，可用于分析驱动栈层面的请求/完成事件，不替代物理层抓包 |
| 专用 USB 协议分析仪（硬件） | 硬件抓包 | 用于 USB 3.x SuperSpeed 物理层/链路层分析，软件抓包无法覆盖的场景（如枚举失败、时序问题） |

## 3. 分析方法论（分层解析）

抓包得到的原始数据应按以下顺序分层解析，逐层对应本目录其他文档：

1. **总线事件层**：设备插入/拔出、复位、地址分配（对应 `USB_Core_Spec.md` 枚举流程）。
2. **Control 传输层**：标准/Class/Vendor 请求，重建 Device/Configuration/Interface/Endpoint/
   Class-Specific 描述符（对应 `USB_Core_Spec.md` 描述符体系）。
3. **Class 协议层**：依据 Interface Descriptor 的 `bInterfaceClass/SubClass/Protocol` 分流到具体
   协议文档（`HID.md` / `MassStorageClass.md` / `AudioVideoClass.md` / `CDC.md`）解析。
4. **应用层**：Class 协议之上的具体设备私有数据（如 HID Vendor-Defined Usage、CDC 透传的业务协议），
   需结合设备厂商文档，超出本仓库通用规范范畴。

## 4. 抓包分析常见坑点

- **枚举时序**：部分设备在 `SET_CONFIGURATION` 之后才会暴露完整 Class-Specific 描述符，抓包需覆盖
  完整枚举过程，而不只是稳定工作后的数据流。
- **Alternate Setting 切换**：音视频类设备的实际数据格式由 `SET_INTERFACE` 决定，遗漏这一事件会导致
  误判数据格式（见 `AudioVideoClass.md`）。
- **多 Report ID 的 HID 设备**：不按 Report ID 分流会导致同一字节偏移在不同 Report 下含义混淆
  （见 `HID.md`）。
- **USB 3.x 链路层状态**：SuperSpeed 设备存在 U0-U3 电源状态切换，软件抓包工具通常只呈现协议层，
  链路层问题需要硬件分析仪配合。

## 5. 参考

- Wireshark USB 抓包官方说明：https://wiki.wireshark.org/CaptureSetup/USB
- USBPcap 项目：https://desowin.org/usbpcap/
- Microsoft ETW USB 相关 Provider 文档：https://learn.microsoft.com/windows-hardware/drivers/usbcon/usb-tracing-and-logging
