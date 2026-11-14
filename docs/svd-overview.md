# SVD 总览（27 个系列）

每份 SVD 对应表中 SDK 内的一份 SVD（内容一致；git 入库时行尾归一为 LF）。设备名和 CPU 标签分别取 XML 顶层
`<name>` 与 `<cpu><name>`。外设数和派生数按顶层 peripheral 统计，中断数取 XML 中
`<interrupt>` 节点数。生成数据时需展开 `derivedFrom`，并用设备头核对中断和内存映射。

| SVD | SDK 来源（`xh_ll_sdk/`） | 设备名 | CPU | 外设数 | 派生外设数 | 中断节点数 |
|---|---|---|---|---:|---:|---:|
| [HC32A448.svd](../svd/HC32A448.svd) | `HC32A448_DDL_Rev1.3.0/drivers/cmsis/Device/XHSC/hc32a4xx/Source/IAR/debugger/HC32A448.svd` | HC32A448 | CM4 | 68 | 20 | 0 |
| [HC32A460.svd](../svd/HC32A460.svd) | `HC32A460_DDL_Rev3.3.0/drivers/cmsis/Device/XHSC/hc32a4xx/Source/IAR/debugger/HC32A460.svd` | HC32A460 | CM4 | 74 | 31 | 0 |
| [HC32A472.svd](../svd/HC32A472.svd) | `HC32A472_DDL_Rev1.4.0/drivers/cmsis/Device/XHSC/hc32a4xx/Source/IAR/debugger/HC32A472.svd` | HC32A472 | CM4 | 98 | 37 | 183 |
| [HC32A4A0.svd](../svd/HC32A4A0.svd) | `HC32A4A0_DDL_Rev2.4.0/drivers/cmsis/Device/HDSC/hc32a4xx/Source/IAR/debugger/HC32A4A0.svd` | HC32A4A0 | CM4 | 123 | 62 | 0 |
| [HC32A4A8.svd](../svd/HC32A4A8.svd) | `HC32A4A8_DDL_Rev1.0.0/drivers/cmsis/Device/XHSC/hc32a4xx/Source/IAR/debugger/HC32A4A8.svd` | HC32A4A8 | CM4 | 134 | 57 | 0 |
| [HC32F115.svd](../svd/HC32F115.svd) | `HC32F115_DDL_Rev1.1.0/drivers/cmsis/Device/HDSC/hc32f1xx/Source/IAR/debugger/HC32F115.svd` | HC32F115 | CM0+ | 29 | 10 | 0 |
| [HC32F120.svd](../svd/HC32F120.svd) | `HC32F120_DDL_Rev2.2.0/drivers/cmsis/Device/HDSC/hc32f1xx/Source/IAR/svd/HC32F120.svd` | HC32F120 | CM0+ | 28 | 10 | 0 |
| [HC32F155.svd](../svd/HC32F155.svd) | `HC32F155_DDL_Rev1.1.0/drivers/cmsis/Device/HDSC/hc32f1xx/Source/IAR/debugger/HC32F155.svd` | HC32F155 | CM0+ | 33 | 12 | 0 |
| [HC32F175.svd](../svd/HC32F175.svd) | `HC32F175_DDL_Rev1.00/drivers/cmsis/Device/HDSC/hc32f1xx/Source/IAR/debugger/HC32F175.svd` | XHSC_HC32F175 | CM0P | 42 | 19 | 0 |
| [HC32F334.svd](../svd/HC32F334.svd) | `HC32F334_DDL_Rev1.1.1/drivers/cmsis/Device/HDSC/hc32f3xx/Source/IAR/debugger/HC32F334.svd` | HC32F334 | CM4 | 66 | 24 | 146 |
| [HC32F336.svd](../svd/HC32F336.svd) | `HC32F336_DDL_Rev1.0.1/drivers/cmsis/Device/HDSC/hc32f3xx/Source/IAR/debugger/HC32F336.svd` | HC32F336 | CM4 | 66 | 24 | 146 |
| [HC32F431.svd](../svd/HC32F431.svd) | `HC32F431_DDL_Rev1.0.1/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F431.svd` | HC32F431 | CM4 | 39 | 9 | 74 |
| [HC32F448.svd](../svd/HC32F448.svd) | `HC32F448_DDL_Rev1.3.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F448.svd` | HC32F448 | CM4 | 68 | 20 | 0 |
| [HC32F451.svd](../svd/HC32F451.svd) | `HC32F451_DDL_Rev3.3.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32f451.svd` | HC32F451 | CM4 | 73 | 31 | 0 |
| [HC32F452.svd](../svd/HC32F452.svd) | `HC32F452_DDL_Rev3.3.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32f452.svd` | HC32F452 | CM4 | 74 | 31 | 0 |
| [HC32F460.svd](../svd/HC32F460.svd) | `HC32F460_DDL_Rev3.3.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F460.svd` | HC32F460 | CM4 | 74 | 31 | 0 |
| [HC32F467.svd](../svd/HC32F467.svd) | `HC32F467_DDL_Rev1.1.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F467.svd` | HC32F467 | CM4 | 102 | 43 | 0 |
| [HC32F472.svd](../svd/HC32F472.svd) | `HC32F472_DDL_Rev1.4.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F472.svd` | HC32F472 | CM4 | 98 | 37 | 183 |
| [HC32F4A0.svd](../svd/HC32F4A0.svd) | `HC32F4A0_DDL_Rev2.4.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F4A0.svd` | HC32F4A0 | CM4 | 123 | 62 | 0 |
| [HC32F4A2.svd](../svd/HC32F4A2.svd) | `HC32F4A2_DDL_Rev2.3.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F4A2.svd` | HC32F4A2 | CM4 | 123 | 62 | 0 |
| [HC32F4A8.svd](../svd/HC32F4A8.svd) | `HC32F4A8_DDL_Rev1.0.0/drivers/cmsis/Device/HDSC/hc32f4xx/Source/IAR/debugger/HC32F4A8.svd` | HC32F4A8 | CM4 | 134 | 57 | 0 |
| [HC32K116.svd](../svd/HC32K116.svd) | `HC32K116_DDL_Rev1.0.0/drivers/cmsis/Device/HDSC/hc32k1xx/Source/IAR/debugger/HC32K116.svd` | HC32K116 | CM0+ | 36 | 4 | 31 |
| [HC32K118.svd](../svd/HC32K118.svd) | `HC32K118_DDL_Rev1.0.0/drivers/cmsis/Device/HDSC/hc32k1xx/Source/IAR/debugger/HC32K118.svd` | HC32K118 | CM0+ | 36 | 4 | 31 |
| [HC32M120.svd](../svd/HC32M120.svd) | `HC32M120_DDL_Rev2.2.0/drivers/cmsis/Device/HDSC/hc32m1xx/Source/IAR/svd/HC32M120.svd` | HC32M120 | CM0+ | 31 | 5 | 0 |
| [HC32M413.svd](../svd/HC32M413.svd) | `HC32M413_DDL_Rev1.0.1/drivers/cmsis/Device/HDSC/hc32m4xx/Source/IAR/debugger/HC32M413.svd` | HC32M413 | CM4 | 39 | 9 | 74 |
| [HC32M441.svd](../svd/HC32M441.svd) | `HC32M441_DDL_Rev1.2.0/drivers/cmsis/Device/HDSC/hc32m4xx/Source/IAR/debugger/HC32M441.svd` | HC32M441 | CM4 | 61 | 19 | 0 |
| [HC32M458.svd](../svd/HC32M458.svd) | `HC32M458_DDL_Rev1.0.0/drivers/cmsis/Device/HDSC/hc32m4xx/Source/IAR/debugger/HC32M458.svd` | HC32M458 | CM4 | 44 | 12 | 79 |

设备头引用 `core_cm0plus.h` 的有 7 个系列，含 F175。其余 20 个系列引用 `core_cm4.h`。
