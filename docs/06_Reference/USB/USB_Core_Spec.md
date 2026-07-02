# USB Core Spec（USB 2.0 / USB 3.x 核心协议）

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 背景与适用范围

USB（Universal Serial Bus）核心规范定义了总线拓扑、电气特性、传输协议与设备框架（Device Framework），
是所有 USB Class 协议（HID / Mass Storage / Audio / Video / CDC 等）的公共基础。抓包分析任何 USB 外设
通信，首先需要理解本篇涉及的传输类型与标准请求/描述符结构。

- USB 2.0：Low-Speed (1.5 Mbps) / Full-Speed (12 Mbps) / High-Speed (480 Mbps)。
- USB 3.x：SuperSpeed (5 Gbps, Gen1) 及以上（Gen2 10 Gbps、Gen2x2 20 Gbps），新增双工数据流与
  Link/Protocol Layer。

## 2. 总线拓扑

- **Host 中心式（Host-centric）**：所有传输由 Host（Host Controller）发起，设备不能主动发起传输。
- 层级结构：Host → Root Hub → (External Hub)* → Device，每个 Device 可包含多个 Interface，每个
  Interface 可包含多个 Endpoint。

## 3. 传输类型（Transfer Types）

抓包分析首先要识别每次数据交换属于哪种传输类型：

| 传输类型 | 特点 | 典型用途 |
| -------- | ---- | -------- |
| Control | 双向、可靠、用于设备枚举/配置，带 Setup/Data/Status 三阶段 | 标准设备请求、Class/Vendor 请求 |
| Bulk | 大数据量、可靠、无固定带宽保证 | 大容量存储、打印机 |
| Interrupt | 小数据量、周期轮询、有限延迟保证 | 键盘/鼠标/HID 输入事件 |
| Isochronous | 固定带宽、允许丢包、无重传 | 音频/视频流（UAC/UVC） |

## 4. 标准设备请求（Standard Device Requests，Control 传输 Setup 阶段）

Setup 包固定 8 字节，字段：`bmRequestType`、`bRequest`、`wValue`、`wIndex`、`wLength`。
常见标准请求（`bRequest`）：

| 请求 | 说明 |
| ---- | ---- |
| GET_DESCRIPTOR / SET_DESCRIPTOR | 读取/设置设备各类描述符 |
| GET_CONFIGURATION / SET_CONFIGURATION | 查询/切换当前配置 |
| GET_INTERFACE / SET_INTERFACE | 查询/切换接口的备用设置（Alternate Setting） |
| SET_ADDRESS | 枚举阶段为设备分配总线地址 |
| CLEAR_FEATURE / SET_FEATURE | 清除/设置特性（如端点 Halt 状态） |
| GET_STATUS | 查询设备/接口/端点状态 |

## 5. 描述符体系（Descriptor Hierarchy）

抓包时枚举阶段的核心是设备返回的一系列描述符，层级如下：

```
Device Descriptor
  └── Configuration Descriptor（可能多个）
        └── Interface Descriptor（可能多个 / 含 Alternate Setting）
              ├── Endpoint Descriptor（每个 Interface 若干个）
              └── Class-Specific Descriptor（由具体 Class 协议定义，如 HID Report Descriptor）
  └── String Descriptor（可选，供人类可读的厂商/产品/序列号字符串）
```

关键字段速览：

- **Device Descriptor**：`idVendor`/`idProduct`（VID/PID，识别设备型号的关键字段）、`bcdUSB`（协议版本）、
  `bDeviceClass/SubClass/Protocol`、`bNumConfigurations`。
- **Configuration Descriptor**：`wTotalLength`、`bNumInterfaces`、`bmAttributes`（自供电/远程唤醒）、
  `bMaxPower`。
- **Interface Descriptor**：`bInterfaceNumber`、`bAlternateSetting`、`bNumEndpoints`、
  `bInterfaceClass/SubClass/Protocol`（决定使用哪个 Class 协议解析后续数据，抓包解析的关键分流字段）。
- **Endpoint Descriptor**：`bEndpointAddress`（方向 + 端点号）、`bmAttributes`（传输类型）、
  `wMaxPacketSize`、`bInterval`（轮询周期，Interrupt/Isochronous 专用）。

## 6. USB 3.x 增量要点

- 新增 **SuperSpeed Endpoint Companion Descriptor**，携带 `bMaxBurst`、`bmAttributes`（Streams 数等）。
- 协议层新增 Link Layer 状态机（U0-U3 电源状态）与 Packet Framing，抓包工具需要额外的分析器
  （如支持 USB 3.x 的硬件分析仪）才能解析物理层细节；协议层的 Control/Bulk/Interrupt/Isochronous
  语义与 USB 2.0 保持兼容，便于软件层复用同一套描述符解析逻辑。

## 7. 参考

- USB-IF 官方规范首页：https://www.usb.org/documents
- USB 2.0 Specification（含 Chapter 9 Device Framework）：https://www.usb.org/document-library/usb-20-specification
- USB 3.2 Specification：https://www.usb.org/document-library/usb-32-specification-released-september-22-2017
- Microsoft USB 驱动开发文档：https://learn.microsoft.com/windows-hardware/drivers/usbcon/
