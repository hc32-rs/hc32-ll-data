# GPIO、调试与中断

GPIO 先登记 F175 的旧式 AFR 控制，再登记端口保护/复用架构。字节端口和 PCR/PFSR 端口是同档不同分支。
F115/F120/F155/M120 的控制窗口在 +0x500；F431/M413 字节端口窗口在 +0x300；
其他 M4 的 16 位交错端口在 +0x3F4 区，K116/K118 为 32 位分块端口。
复用功能由 hc32_ll_gpio.h 的 GPIO_FUNC_* 与例程核对，封装级完整映射仍依赖芯片资料。

DBGC 从停止控制、状态/停机控制、跟踪控制到 Flash CRC 控制分档。
DBGC_T 是认证/目标控制窗口，保留独立 dbgc_t 名称。
INTC 从固定向量、部分可路由槽到 144 槽分档；同档位图和 EIRQ 数量分变体。
事件端口由 AOS 承载，没有独立生成块时不额外创建 event_port 版本。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### dbgc：v1

MCU 调试状态和停止控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dbgc_ll_v1.rs` | [dbgc_ll_v1](../../data/registers/dbgc_ll_v1.yaml) | K116/K118: DBGC | 2 / 11 | 本档位对照基准；完整定义见来源 YAML |
| `dbgc_ll_v1_1.rs` | [dbgc_ll_v1_1](../../data/registers/dbgc_ll_v1_1.yaml) | F115/F155: DBGC | 2 / 14 | 相对 dbgc_ll_v1：定义差异 MCUSTPCTL |
| `dbgc_ll_v1_2.rs` | [dbgc_ll_v1_2](../../data/registers/dbgc_ll_v1_2.yaml) | F120: DBGC | 2 / 15 | 相对 dbgc_ll_v1：定义差异 MCUSTPCTL |
| `dbgc_ll_v1_3.rs` | [dbgc_ll_v1_3](../../data/registers/dbgc_ll_v1_3.yaml) | M120: DBGC | 2 / 16 | 相对 dbgc_ll_v1：定义差异 MCUSTPCTL |

### dbgc：v2

运行/停止调试与跟踪控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dbgc_ll_v2.rs` | [dbgc_ll_v2](../../data/registers/dbgc_ll_v2.yaml) | A460/F451/F452/F460: DBGC | 3 / 21 | 本档位对照基准；完整定义见来源 YAML |
| `dbgc_ll_v2_1.rs` | [dbgc_ll_v2_1](../../data/registers/dbgc_ll_v2_1.yaml) | A4A0/F467/F4A0/F4A2: DBGC | 4 / 36 | 相对 dbgc_ll_v2：增加 MCUDBGCSTAT/MCUSTPCTL2；缺少 MCUDBGSTAT；定义差异 MCUSTPCTL |
| `dbgc_ll_v2_2.rs` | [dbgc_ll_v2_2](../../data/registers/dbgc_ll_v2_2.yaml) | A448/F448/M441: DBGC | 5 / 19 | 相对 dbgc_ll_v2：增加 CHIPID/MCUDBGCSTAT/MCUSTPCTL2；缺少 MCUDBGSTAT；定义差异 MCUSTPCTL |
| `dbgc_ll_v2_3.rs` | [dbgc_ll_v2_3](../../data/registers/dbgc_ll_v2_3.yaml) | F334/F336: DBGC | 5 / 25 | 相对 dbgc_ll_v2：增加 CHIPID/MCUDBGCSTAT/MCUSTPCTL2；缺少 MCUDBGSTAT；定义差异 MCUSTPCTL |
| `dbgc_ll_v2_4.rs` | [dbgc_ll_v2_4](../../data/registers/dbgc_ll_v2_4.yaml) | A472/F472: DBGC | 5 / 30 | 相对 dbgc_ll_v2：增加 CHIPID/MCUDBGCSTAT/MCUSTPCTL2；缺少 MCUDBGSTAT；定义差异 MCUSTPCTL |
| `dbgc_ll_v2_5.rs` | [dbgc_ll_v2_5](../../data/registers/dbgc_ll_v2_5.yaml) | A4A8/F4A8: DBGC | 5 / 42 | 相对 dbgc_ll_v2：增加 CHIPID/MCUDBGCSTAT/MCUSTPCTL2；缺少 MCUDBGSTAT；定义差异 MCUSTPCTL |

### dbgc：v3

Flash CRC 调试控制；是否含 MCU 状态按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dbgc_ll_v3.rs` | [dbgc_ll_v3](../../data/registers/dbgc_ll_v3.yaml) | F431/M413: DBGC | 5 / 9 | 本档位对照基准；完整定义见来源 YAML |
| `dbgc_ll_v3_1.rs` | [dbgc_ll_v3_1](../../data/registers/dbgc_ll_v3_1.yaml) | M458: DBGC | 7 / 25 | 相对 dbgc_ll_v3：增加 MCUDBGCSTAT/MCUSTPCTL |

### dbgc_t：v1

调试认证/目标控制，ID 与保护字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dbgc_t_ll_v1.rs` | [dbgc_t_ll_v1](../../data/registers/dbgc_t_ll_v1.yaml) | F155/M120: DBGC_T | 7 / 12 | 本档位对照基准；完整定义见来源 YAML |
| `dbgc_t_ll_v1_1.rs` | [dbgc_t_ll_v1_1](../../data/registers/dbgc_t_ll_v1_1.yaml) | K116/K118: DBGC_T | 8 / 17 | 相对 dbgc_t_ll_v1：增加 CHIPID/TARGETID；缺少 RESV0；定义差异 MCUSTAT |

### gpio：v1

端口保护与复用；字节端口和 PCR/PFSR 为同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `gpio_ll_v1.rs` | [gpio_ll_v1](../../data/registers/gpio_ll_v1.yaml) | F120: GPIO | 110 / 695 | 本档位对照基准；完整定义见来源 YAML |
| `gpio_ll_v1_1.rs` | [gpio_ll_v1_1](../../data/registers/gpio_ll_v1_1.yaml) | F115: GPIO | 110 / 983 | 相对 gpio_ll_v1：定义差异 PIDR0/PIDR12/PIDR13/PIDR14/PIDR3 等 54 项 |
| `gpio_ll_v1_2.rs` | [gpio_ll_v1_2](../../data/registers/gpio_ll_v1_2.yaml) | M120: GPIO | 114 / 763 | 相对 gpio_ll_v1：增加 PCR130/PCR140/PCR74/PCR75；定义差异 PIDR13/PIDR14/PIDR7/PODR13/PODR14 等 18 项 |
| `gpio_ll_v1_3.rs` | [gpio_ll_v1_3](../../data/registers/gpio_ll_v1_3.yaml) | F155: GPIO | 128 / 1239 | 相对 gpio_ll_v1：增加 PCR02/PCR03/PCR04/PCR05/PCR06 等 18 项；定义差异 PCR00/PCR01/PCR10/PCR11/PCR12 等 94 项 |
| `gpio_ll_v1_4.rs` | [gpio_ll_v1_4](../../data/registers/gpio_ll_v1_4.yaml) | M458: GPIO | 137 / 1097 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 134 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 107 项；定义差异 PCCR/PSPCR/PWPR |
| `gpio_ll_v1_5.rs` | [gpio_ll_v1_5](../../data/registers/gpio_ll_v1_5.yaml) | F334/F336: GPIO | 138 / 1289 | 相对 gpio_ll_v1：增加 HRPWM/PCRA0/PCRA1/PCRA10/PCRA11 等 135 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 107 项；定义差异 PCCR/PSPCR/PWPR |
| `gpio_ll_v1_6.rs` | [gpio_ll_v1_6](../../data/registers/gpio_ll_v1_6.yaml) | K116/K118: GPIO | 150 / 1358 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 146 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 106 项；定义差异 PCCR/PINAER/PSPCR/PWPR |
| `gpio_ll_v1_7.rs` | [gpio_ll_v1_7](../../data/registers/gpio_ll_v1_7.yaml) | A448/F448/M441: GPIO | 173 / 1586 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 170 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 107 项；定义差异 PCCR/PSPCR/PWPR |
| `gpio_ll_v1_8.rs` | [gpio_ll_v1_8](../../data/registers/gpio_ll_v1_8.yaml) | F431/M413: GPIO | 180 / 1216 | 相对 gpio_ll_v1：增加 PCR02/PCR32/PCR33/PCR36/PCR37 等 118 项；缺少 PCR12/PCR120/PCR121/PCR122/PCR123 等 48 项；定义差异 PCCR/PCR00/PCR01/PCR10/PCR11 等 50 项 |
| `gpio_ll_v1_9.rs` | [gpio_ll_v1_9](../../data/registers/gpio_ll_v1_9.yaml) | A460/F451/F452/F460: GPIO | 206 / 1500 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 202 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 106 项；定义差异 PCCR/PINAER/PSPCR/PWPR |
| `gpio_ll_v1_10.rs` | [gpio_ll_v1_10](../../data/registers/gpio_ll_v1_10.yaml) | A472/F472: GPIO | 217 / 1916 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 214 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 107 项；定义差异 PCCR/PSPCR/PWPR |
| `gpio_ll_v1_11.rs` | [gpio_ll_v1_11](../../data/registers/gpio_ll_v1_11.yaml) | F467: GPIO | 290 / 2210 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 286 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 106 项；定义差异 PCCR/PINAER/PSPCR/PWPR |
| `gpio_ll_v1_12.rs` | [gpio_ll_v1_12](../../data/registers/gpio_ll_v1_12.yaml) | A4A0/F4A0/F4A2: GPIO | 342 / 2704 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 338 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 106 项；定义差异 PCCR/PINAER/PSPCR/PWPR |
| `gpio_ll_v1_13.rs` | [gpio_ll_v1_13](../../data/registers/gpio_ll_v1_13.yaml) | A4A8/F4A8: GPIO | 342 / 2716 | 相对 gpio_ll_v1：增加 PCRA0/PCRA1/PCRA10/PCRA11/PCRA12 等 338 项；缺少 PCR00/PCR01/PCR10/PCR11/PCR12 等 106 项；定义差异 PCCR/PINAER/PSPCR/PWPR |

### intc：v1

固定具名向量及外部中断控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `intc_ll_v1.rs` | [intc_ll_v1](../../data/registers/intc_ll_v1.yaml) | M458: INTC | 18 / 187 | 本档位对照基准；完整定义见来源 YAML |
| `intc_ll_v1_1.rs` | [intc_ll_v1_1](../../data/registers/intc_ll_v1_1.yaml) | K116/K118: INTC | 23 / 106 | 相对 intc_ll_v1：增加 FPRCR/INTEN0/INTEN1/INTEN2/INTEN3 等 8 项；缺少 FPUIER/IER/SWIER；定义差异 EIFCR/EIFR/EIRQCR0/EIRQCR1/EIRQCR2 等 15 项 |

### intc：v2

部分通用路由槽与具名向量混合。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `intc_ll_v2.rs` | [intc_ll_v2](../../data/registers/intc_ll_v2.yaml) | F120: INTC | 44 / 203 | 本档位对照基准；完整定义见来源 YAML |
| `intc_ll_v2_1.rs` | [intc_ll_v2_1](../../data/registers/intc_ll_v2_1.yaml) | F115: INTC | 44 / 205 | 相对 intc_ll_v2：定义差异 WUPEN |
| `intc_ll_v2_2.rs` | [intc_ll_v2_2](../../data/registers/intc_ll_v2_2.yaml) | M120: INTC | 44 / 206 | 相对 intc_ll_v2：定义差异 WUPEN |
| `intc_ll_v2_3.rs` | [intc_ll_v2_3](../../data/registers/intc_ll_v2_3.yaml) | F155: INTC | 45 / 207 | 相对 intc_ll_v2：增加 EIRQCR10/EIRQCR11；缺少 NMICR；定义差异 EIRQCLR/EIRQFR/EKEYCR/NMICLR/NMIENR 等 7 项 |
| `intc_ll_v2_4.rs` | [intc_ll_v2_4](../../data/registers/intc_ll_v2_4.yaml) | F431/M413: INTC | 51 / 199 | 相对 intc_ll_v2：增加 EIFCR/EIFR/FPUIER/IER/INTEN0 等 40 项；缺少 EIRQCLR/EIRQCR8/EIRQCR9/EIRQFR/EKEYCR 等 33 项；定义差异 EIRQCR0/EIRQCR1/EIRQCR2/EIRQCR3/EIRQCR4 等 11 项 |
| `intc_ll_v2_5.rs` | [intc_ll_v2_5](../../data/registers/intc_ll_v2_5.yaml) | A448/F448/M441: INTC | 57 / 275 | 相对 intc_ll_v2：增加 EIFCR/EIFR/EIRQCR10/EIRQCR11/EIRQCR12 等 45 项；缺少 EIRQCLR/EIRQFR/EKEYCR/FPRCR/ISELAR10 等 32 项；定义差异 EIRQCR0/EIRQCR1/EIRQCR2/EIRQCR3/EIRQCR4 等 12 项 |
| `intc_ll_v2_6.rs` | [intc_ll_v2_6](../../data/registers/intc_ll_v2_6.yaml) | A472/F472: INTC | 57 / 280 | 相对 intc_ll_v2：增加 EIFCR/EIFR/EIRQCR10/EIRQCR11/EIRQCR12 等 45 项；缺少 EIRQCLR/EIRQFR/EKEYCR/FPRCR/ISELAR10 等 32 项；定义差异 EIRQCR0/EIRQCR1/EIRQCR2/EIRQCR3/EIRQCR4 等 12 项 |
| `intc_ll_v2_7.rs` | [intc_ll_v2_7](../../data/registers/intc_ll_v2_7.yaml) | F334/F336: INTC | 58 / 282 | 相对 intc_ll_v2：增加 EIFCR/EIFR/EIRQCR10/EIRQCR11/EIRQCR12 等 46 项；缺少 EIRQCLR/EIRQFR/EKEYCR/FPRCR/ISELAR10 等 32 项；定义差异 EIRQCR0/EIRQCR1/EIRQCR2/EIRQCR3/EIRQCR4 等 12 项 |

### intc：v3

144 个通用路由槽及外部中断控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `intc_ll_v3.rs` | [intc_ll_v3](../../data/registers/intc_ll_v3.yaml) | A460/F451/F452/F460: INTC | 170 / 856 | 本档位对照基准；完整定义见来源 YAML |
| `intc_ll_v3_1.rs` | [intc_ll_v3_1](../../data/registers/intc_ll_v3_1.yaml) | A4A0/F467/F4A0/F4A2: INTC | 170 / 872 | 相对 intc_ll_v3：增加 NOCCR；缺少 NMICR；定义差异 EIRQCR0/EIRQCR1/EIRQCR10/EIRQCR11/EIRQCR12 等 20 项 |
| `intc_ll_v3_2.rs` | [intc_ll_v3_2](../../data/registers/intc_ll_v3_2.yaml) | A4A8/F4A8: INTC | 171 / 897 | 相对 intc_ll_v3：增加 FPUIER/INTSEL0/INTSEL1/INTSEL10/INTSEL100 等 133 项；缺少 NMICFR/NMICR/NMIENR/SEL0/SEL1 等 132 项；定义差异 EIRQCR0/EIRQCR1/EIRQCR10/EIRQCR11/EIRQCR12 等 17 项 |
<!-- peripheral-versions:end -->

## IRQ 向量

| 系列 | 设备头 `IRQn_Type` |
|---|---|
| F115/F120/F155/M120 | 外部中断占前 8 个槽，通用槽为 `INT008_IRQn..INT031_IRQn` |
| K116/K118 | 具名固定向量，多个外设或端口可共享同一向量 |
| F175 | 具名固定向量，如 `DMAC_IRQn=7` |
| A460/F451/F452/F460/F467/A4A0/A4A8/F4A0/F4A2/F4A8 | 144 个通用槽 `INT000_IRQn..INT143_IRQn` |
| A448/A472/F334/F336/F448/F472/M441/F431/M413 | 0–15 通用槽，16–31 为具名外部端口中断，其后为具名外设向量 |
| M458 | 固定具名向量，前 8 个为外部端口中断 |
