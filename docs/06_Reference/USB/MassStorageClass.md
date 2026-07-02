# Mass Storage Class（MSC）

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 背景与适用范围

Mass Storage Class（`bInterfaceClass = 0x08`）用于 U 盘、外置硬盘、读卡器等存储类设备。绝大多数消费级
设备使用其中的 **Bulk-Only Transport (BOT)** 子类（`bInterfaceSubClass = 0x06`，`bInterfaceProtocol = 0x50`），
本篇聚焦 BOT + SCSI 命令集这一最常见组合。

## 2. 传输模型：Bulk-Only Transport (BOT)

仅使用两个 Bulk 端点（Bulk-IN / Bulk-OUT），每次命令交互固定为三段式：

```
CBW（Command Block Wrapper，Host → Device，Bulk-OUT，31 字节）
  → Data（可选，方向由 CBW 决定，Bulk-IN 或 Bulk-OUT）
    → CSW（Command Status Wrapper，Device → Host，Bulk-IN，13 字节）
```

### CBW 关键字段

| 字段 | 说明 |
| ---- | ---- |
| dCBWSignature | 固定魔数 `0x43425355`（"USBC"），用于抓包时快速定位命令边界 |
| dCBWTag | 命令标签，Host 生成，Device 原样回传于 CSW，供匹配请求/响应 |
| dCBWDataTransferLength | 预期传输的数据字节数 |
| bmCBWFlags | 数据方向位（Bit 7：0=Out, 1=In） |
| bCBWLUN | 目标逻辑单元号 |
| bCBWCBLength / CBWCB | 实际 SCSI 命令块长度与内容（如 INQUIRY / READ(10) / WRITE(10) 等） |

### CSW 关键字段

| 字段 | 说明 |
| ---- | ---- |
| dCSWSignature | 固定魔数 `0x53425355`（"USBS"） |
| dCSWTag | 需与对应 CBW 的 `dCBWTag` 一致 |
| dCSWDataResidue | 未传输完成的剩余字节数 |
| bCSWStatus | 0=成功，1=失败，2=Phase Error（阶段错误，通常需要 Bulk-Only Mass Storage Reset 恢复） |

## 3. 常见 SCSI 命令（封装于 CBWCB 中）

| 命令 | 操作码 | 说明 |
| ---- | ------ | ---- |
| INQUIRY | 0x12 | 查询设备类型/厂商信息，枚举后常见的第一条命令 |
| READ CAPACITY (10) | 0x25 | 查询容量与块大小 |
| READ (10) | 0x28 | 读取数据块 |
| WRITE (10) | 0x2A | 写入数据块 |
| TEST UNIT READY | 0x00 | 探测介质是否就绪（如检测 U 盘是否插入） |
| REQUEST SENSE | 0x03 | 获取上一次命令失败的详细错误信息 |

## 4. 抓包分析要点

1. 先按 `dCBWSignature`/`dCSWSignature` 魔数在字节流中定位 CBW/CSW 边界。
2. 用 `dCBWTag` 将 CBW 与对应 CSW 配对，中间的数据段按 `bmCBWFlags` 方向位与
   `dCBWDataTransferLength` 长度截取。
3. 解析 CBWCB 内的 SCSI 操作码，进一步按 SCSI Primary Commands (SPC) / SCSI Block Commands (SBC)
   规范解读具体命令参数。

## 5. 参考

- USB Mass Storage Class 规范（Bulk-Only Transport）：https://www.usb.org/document-library/mass-storage-bulk-only-10
- USB Mass Storage Class 规范总览：https://www.usb.org/document-library/mass-storage
- T10 SCSI 命令集规范（SPC/SBC）：https://www.t10.org/
