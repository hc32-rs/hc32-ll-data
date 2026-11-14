# hc32-ll-data

小华半导体 HC32 LL 驱动系列的寄存器数据与外设分类资料。覆盖 **27 个系列**，
包括 7 个 Cortex-M0+ 系列和 20 个 Cortex-M4 系列。
HC32F175 的设备头引用 Cortex-M0+ 内核头。它使用 LL 接口，多个外设沿用旧式寄存器布局。

## 资料

- [SVD 与 SDK 对照表](docs/svd-overview.md)列出本仓库的系列、SDK 版本与 SVD 元数据。
- [驱动矩阵](docs/driver-matrix.json)按 SDK 的 `drivers/hc32_ll_driver/inc/*.h` 生成。
  值为系列名与 SDK 修订号的拼接。LL 矩阵保留原 JSON 的 `ll_hc32_ll_*` 键格式。
- [外设 IP 分类](docs/ip-classification.md)按外设组链接分类、寄存器布局和 SDK 核验依据。
- [布局编号与功能档位](docs/layout-versions.md)记录功能档位和旧编号对照。
- [SDK 核验记录](docs/sdk-verification.md)列出逐组证据及待确认项。

SDK 源码在工作区相邻的 `../xh_ll_sdk/`。设备头、驱动和例程用于核对 SVD 中的寄存器、
中断、时钟门控与触发源。当前阶段已归档原始 SVD 和分类文档。

## 数据目录规划

后续 YAML 用于生成 Rust PAC 并为 HAL 提供实例级数据。布局沿用原项目的目录规划。

| 目录 | 用途 |
|---|---|
| `data/chips/` | 型号、封装、内存与系列引用 |
| `data/family/` | 系列共享外设 |
| `data/peripherals/` | 外设实例、时钟门控、引脚功能和 DMA 请求 |
| `data/registers/` | 按 `<kind>_<version>.yaml` 命名的寄存器块 |
| `data/interrupts/` | 中断向量 |
| `data/dma/` | DMA 通道与请求映射 |
| `data/memory/` | 链接脚本使用的内存布局 |

## M4 扩展范围

原 M4 调研重点是与 M0/M0+ 重叠的外设。DCU、MAU、FMAC、HASH、SKE、USB、ETH、MDIO、
NFC、SDIOC、DVP、QSPI、SMC、DMC、MPU、KEYSCAN、OTS、VREF、HRPWM、HALL、PLA、
TMR6、ERMU 及 M458 的 DFM 仍待单独分类。这里的 MPU 指 SDK 配置的片上总线存储保护单元。
各驱动的 SDK 分布见矩阵。
