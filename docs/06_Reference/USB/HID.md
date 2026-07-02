# HID（Human Interface Device Class）

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 背景与适用范围

HID 是使用最广泛的 USB Class 协议之一，覆盖键盘、鼠标、游戏手柄、触控板，以及大量自定义人机交互/
传感器外设（因为其"自描述数据格式"特性，很多非标准输入设备也复用 HID 协议以避免编写专用驱动）。
识别到 Interface Descriptor 的 `bInterfaceClass = 0x03` 即可判定为 HID 设备，抓包时应转入本篇解析。

## 2. 关键组成

- **HID Descriptor**（Class-Specific Descriptor）：描述该接口使用的 HID 版本号、Country Code，
  以及关联的 Report Descriptor 长度。
- **Report Descriptor**：一段自描述的字节码（而非固定 C 结构体），通过 GET_DESCRIPTOR（Class 请求）
  读取，用 Item（Usage Page / Usage / Report Size / Report Count / Input / Output / Feature 等）
  声明数据字段的语义和布局。抓包分析 HID 设备必须先获取并解析 Report Descriptor，才能正确解读
  后续 Interrupt 传输中的原始字节含义。
- **Report**：实际传输的数据包，分三类：
  - `Input Report`：设备 → 主机（如按键状态）。
  - `Output Report`：主机 → 设备（如键盘 LED 状态）。
  - `Feature Report`：双向、非周期性的配置类数据。

## 3. 传输模型

- 通常使用 **Interrupt IN** 端点周期性上报 Input Report（轮询周期由 Endpoint Descriptor 的
  `bInterval` 决定）。
- 也可通过 **Control 传输**的 Class 请求（`GET_REPORT` / `SET_REPORT` / `GET_IDLE` / `SET_IDLE` /
  `GET_PROTOCOL` / `SET_PROTOCOL`）读写 Report，常用于 Feature Report 或不要求低延迟的场景。

## 4. Report Descriptor 关键 Item 速览

| Item | 作用 |
| ---- | ---- |
| Usage Page / Usage | 声明数据的语义类别（如 Generic Desktop / Keyboard / Mouse / Vendor-Defined） |
| Report Size / Report Count | 声明每个字段的位宽与重复次数，决定如何按位切分原始字节流 |
| Logical Minimum/Maximum | 声明数值的合法范围，用于校验/换算 |
| Input / Output / Feature | 声明该字段属于哪类 Report，以及数据/常量、绝对/相对等属性位 |
| Collection / End Collection | 对字段分组（如区分多个逻辑设备） |

## 5. 抓包分析要点

1. 先在枚举阶段抓到完整 Report Descriptor（GET_DESCRIPTOR, Type=HID Report），逐 Item 还原字段布局。
2. 再对 Interrupt IN 数据按 Report Descriptor 定义的位偏移/位宽切分，还原出具体按键/坐标/自定义字段值。
3. 若设备有多个 Report ID（Report Descriptor 中出现 `Report ID` Item），首字节为 Report ID，
   需按 ID 分流解析不同结构。

## 6. 参考

- USB HID 官方规范：https://www.usb.org/hid
- HID Usage Tables：https://www.usb.org/document-library/hid-usage-tables-14
- Microsoft HID 驱动文档：https://learn.microsoft.com/windows-hardware/drivers/hid/
