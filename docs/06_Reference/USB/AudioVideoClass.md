# Audio / Video Class（UAC / UVC）

> Status: Draft
>
> Last Updated: 2026-07-02

---

## 1. 背景与适用范围

- **USB Audio Class (UAC)**（`bInterfaceClass = 0x01`）：USB 声卡、麦克风、耳机等音频设备的标准协议，
  常见 UAC 1.0 与 UAC 2.0（USB 2.0 High-Speed 起支持更高带宽/采样率与更灵活的时钟拓扑）。
- **USB Video Class (UVC)**（`bInterfaceClass = 0x0E`）：USB 摄像头等视频设备的标准协议。

两者都大量使用 **Isochronous 传输**（允许丢包、保证固定带宽，适合实时音视频流），并通过
Class-Specific Descriptor 描述其内部功能拓扑（Unit/Terminal 图）。

## 2. UAC 关键组成

- **Audio Control (AC) Interface**：描述音频功能拓扑，由若干 **Unit/Terminal** 组成的有向图：
  - `Input Terminal` / `Output Terminal`：音频信号的输入/输出端点（如麦克风、扬声器、USB 流本身）。
  - `Feature Unit`：音量、静音等可控特性节点。
  - `Mixer Unit` / `Selector Unit`：混音/选择节点。
- **Audio Streaming (AS) Interface**：实际承载音频数据流的 Interface，通常有多个 Alternate Setting
  （Alt 0 = 零带宽/空闲，Alt N = 特定采样率/声道数的数据流配置）。
- **Isochronous Audio Data Endpoint Descriptor**：声明采样率同步方式（Synchronous / Asynchronous /
  Adaptive），抓包时用于判断时钟恢复机制。

## 3. UVC 关键组成

- **Video Control (VC) Interface**：描述视频功能拓扑（Input/Output Terminal、Processing Unit、
  Extension Unit 等），类似 UAC 的 Unit/Terminal 图。
- **Video Streaming (VS) Interface**：承载视频流，包含：
  - `Format Descriptor`（如 Uncompressed/MJPEG/Frame-Based）声明编码格式。
  - `Frame Descriptor`：声明具体分辨率、帧率范围。
- **Video Probe and Commit Control**：Host 通过 Class 请求（`SET_CUR`/`GET_CUR` 等）与设备协商
  实际使用的格式/分辨率/帧率，协商结果通过 Alternate Setting 对应的 Isochronous 带宽承载。

## 4. 抓包分析要点

1. 枚举阶段先解析 AC/VC Interface 的 Class-Specific Descriptor，还原 Unit/Terminal 拓扑图，
   明确每个 Streaming Interface 对应的功能。
2. 关注 Alternate Setting 切换（`SET_INTERFACE`）——这是设备从"空闲"进入"实际按某种格式流传输"的
   关键事件，抓包时应重点标记。
3. Isochronous 数据段本身通常是压缩/未压缩的媒体数据（如 MJPEG 帧、PCM 采样），协议分析层面只需
   正确切分 Packet 边界与格式头（如 UVC 每帧起始的 Payload Header），媒体内容解码超出协议抓包范畴。

## 5. 参考

- USB Audio Class 规范：https://www.usb.org/document-library/audio-devices-rev-30-and-adopters-agreement
- USB Video Class 规范：https://www.usb.org/document-library/video-class-v15-document-set
- Microsoft USB Audio/Video 驱动文档：https://learn.microsoft.com/windows-hardware/drivers/audio/ 、
  https://learn.microsoft.com/windows-hardware/drivers/stream/
