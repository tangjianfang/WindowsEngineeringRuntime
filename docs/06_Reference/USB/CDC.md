# CDC（Communications Device Class）

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 背景与适用范围

CDC（`bInterfaceClass = 0x02`）用于将 USB 设备模拟为通信设备，最常见的子类是 **CDC-ACM
（Abstract Control Model）**——把 USB 设备虚拟为标准串口（Virtual COM Port），广泛用于：

- 单片机/嵌入式开发板（Arduino、STM32 等）的调试串口。
- 调制解调器、GNSS 模块等传感器/外设的控制通道。
- 与本项目相关性：很多"外设交互通信"调试场景（如设备日志输出、AT 指令交互）都经由 CDC-ACM
  虚拟串口完成，是 Windows Native 工程中排查外设问题时最常遇到的协议之一。

## 2. 接口组成

CDC-ACM 设备通常声明两个 Interface（可合并为一个 Interface Association Descriptor, IAD）：

- **Communication Interface（Class = 0x02, SubClass = 0x02 ACM）**：
  - 使用一个 Interrupt IN 端点上报状态变化（`SERIAL_STATE` 通知）。
  - 通过 Control 传输的 Class 请求管理线路状态：
    - `SET_LINE_CODING` / `GET_LINE_CODING`：设置/查询波特率、数据位、校验位、停止位。
    - `SET_CONTROL_LINE_STATE`：设置 DTR/RTS。
    - `SEND_BREAK`：发送 Break 信号。
- **Data Interface（Class = 0x0A，CDC-Data）**：
  - 一对 Bulk IN/OUT 端点，承载实际的串行数据字节流（即"用户看到的串口收发内容"）。

## 3. 关键结构

- **Line Coding 结构**（7 字节，`SET_LINE_CODING`/`GET_LINE_CODING` 数据阶段）：
  `dwDTERate`（波特率）、`bCharFormat`（停止位）、`bParityType`（校验位）、`bDataBits`（数据位）。
- **Notification Header**（Interrupt IN 状态上报）：与 Control 传输 Setup 包格式类似，
  `bNotificationCode = SERIAL_STATE (0x20)` 时携带 `UART_STATE`（DCD/DSR/Break/Ring 等状态位）。

## 4. 抓包分析要点

1. 先确认 Interface 上的 IAD（若存在）与 Class/SubClass，判断是否为 CDC-ACM。
2. Data Interface 的 Bulk IN/OUT 内容即为"透传"的原始串行数据，与业务协议（如 AT 指令、
   自定义二进制协议）的解析是下一层工作，需结合具体设备的应用层协议文档。
3. 关注 `SET_LINE_CODING` 的参数变化，判断主机侧对波特率等串口参数的实际配置，
   有助于排查"设备无响应"是否为参数不匹配所致。

## 5. 参考

- USB CDC 规范（含 ACM 子类）：https://www.usb.org/document-library/class-definitions-communication-devices-12
- Microsoft USB CDC-ACM 驱动文档：https://learn.microsoft.com/windows-hardware/drivers/usbcon/usb-com-port-devices
