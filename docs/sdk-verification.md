# LL SDK 核验

范围为工作区 `xh_ll_sdk/` 下 27 套 SDK。设备头在
`drivers/cmsis/Device/*/*/Include/`，LL 头文件在
`drivers/hc32_ll_driver/inc/`。27 份归档 SVD 与相应 SDK 内 IAR、GCC
各一份 SVD 在初始归档时内容一致（行尾归一为 LF）；后续修订以 [CHANGELOG](../svd/CHANGELOG.md) 为准。按 SDK 的头文件清单重新枚举后，与
[驱动矩阵](driver-matrix.json)一致。逐套设备头确认 7 个 Cortex-M0+
和 20 个 Cortex-M4，F175 使用 `core_cm0plus.h`。

| 外设组 | 已复核内容 |
|---|---|
| [模拟](explore/analog.md) | ADC SVD 布局分组，F155/K116/K118/M120 的 ADC→AOS→DMA 例程，F175 的 DMAC 请求宏 |
| [时钟与复位](explore/clock-reset.md) | 27 套 FCG 门控源，CTC/FCM 的 SVD 与设备头掩码 |
| [电源与看门狗](explore/power-wdt.md) | PWC 布局分组，M4 全部 WDT/SWDT 的寄存器与位域 |
| [GPIO 与中断](explore/gpio-int.md) | 27 套设备头 IRQn 形式，GPIO/INTC/DBGC 的 SVD 分布 |
| [串口](explore/serial.md) | USART 派生实例及位域签名，M4 的 18 个候选布局 |
| [SPI、I2C 与 I2S](explore/spi-i2c-i2s.md) | SPI 两组、I2C 两组、I2S 四组，F460 的 SPI.DR 和 K116/F460 的 I2C.SLTR |
| [高级定时器](explore/timer-adv.md) | F175 ATIM3 的地址和驱动，EMB 布局及 A472/F472 的多余节点 |
| [通用定时器](explore/timer-gen.md) | TMR4 的跨系列布局，F155 TMRB 与 M4 TMRA 数据寄存器的设备头掩码 |
| [AOS](explore/pca-bt-pcnt-aos.md) | 13 个 SVD 布局组、基址与寄存器数，M458 的 AOS 驱动及路由目标 |
| [DMA](explore/dma.md) | 控制器布局、固定/动态中断、F460 SWREQ 掩码与通道数 |
| [存储](explore/memory.md) | EFM/SRAM 控制器的 SVD 分组，27 套 SDK 常规 GCC 链接脚本的 Flash/RAM 配置 |
| [CRC、RTC 与通信](explore/crypto-comm.md) | CRC、TRNG、RTC、CAN/MCAN 的实例与布局分组 |

SVD 布局比较使用寄存器名、偏移、位域名和位位置，并解析外设级 `derivedFrom`。
各组命名已统一为[已启用命名](layout-versions.md)。此次映射按完整现有生成布局保留变体，
不将旧文档静态签名视为可直接合并的 IR；访问属性、复位值和可选位仍依来源核对。
本次验证使用 SDK 文件，封装级引脚复用、完整料号容量和硬件运行行为仍需手册或实测。
