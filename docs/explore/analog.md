# ADC、DAC 与比较器

LL 从自身基础档位编号，不沿用 DDL 的主编号。
ADC 从 F175 的 SQR/JQR 四 rank、M0+ 的 SeqA/SeqB 统一采样时间，发展到逐通道采样，
再区分增益/保持/运放扩展与差分 DIR 结果读出。OPACR、PGA、SHCR 属同档不同分支，
同步、平均、通道和结果数量均按实例保留。静态 SVD 签名不能代替完整生成 IR。

F155/K116/K118/M120 的 `projects/*/examples/adc/adc_dma/source/main.c` 使用
AOS_SetTriggerEventSrc 将 EVT_SRC_ADC_EOCA 路由到 DMA。F115/F120 无 DMA。
F175 的请求见 `hc32_ll_dmac.h` 中 DMA_ADC_JRQ_DREQ/DMA_ADC_EOC_DREQ。

CMP 从 mini 控制、窗口/消隐、同步控制到内嵌 DAC 分档。TWSR/TWPR、BWSR/BWPR、
TWR 及 VISR 有无均保留不同布局；CMP_COMMON 是共享 DAC 窗口，不与通道实例混为一体。
DAC 的基本数据窗口与增益/对齐变体之后是 F334/F336 的 DAC-ADC 硬件触发控制。
A472/F472 各四个 DAC，A448/F448/M441 各一个，其余已列 DAC 系列各两个。

F431/M413/M458 ADC/CMP 门控在 CMU.FCG，其余 M4 LL 系列在 PWC.FCG3。
A448 的 ADC1_IRQn=128、CMP1_IRQn=89。IRQn_Type 与 en_event_src_t 分别提供中断与 AOS 事件号。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### adc：v1

旧式 SQR/JQR 四 rank 序列、无内嵌 BGR。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ll_v1.rs` | [adc_v17](../../data/registers/adc_v17.yaml) | F175: ADC | 26 / 81 | 本档位对照基准；完整定义见来源 YAML |

### adc：v2

SeqA/SeqB、统一采样时间；通道数及 OPACR 分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ll_v2.rs` | [adc_v15](../../data/registers/adc_v15.yaml) | F115/F120: ADC | 32 / 34 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ll_v2_1.rs` | [adc_v24](../../data/registers/adc_v24.yaml) | M120: ADC | 33 / 36 | 相对 adc_ll_v2：增加 OPACR |
| `adc_ll_v2_2.rs` | [adc_v16](../../data/registers/adc_v16.yaml) | F155: ADC | 34 / 34 | 相对 adc_ll_v2：增加 DR12/DR13；定义差异 CHSELRA/CHSELRB |
| `adc_ll_v2_3.rs` | [adc_v23](../../data/registers/adc_v23.yaml) | K116/K118: ADC | 36 / 34 | 相对 adc_ll_v2：增加 DR12/DR13/DR14/DR15；定义差异 CHSELRA/CHSELRB |

### adc：v3

逐通道采样与多结果，平均/同步/看门狗位域按实例保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ll_v3.rs` | [adc_v5](../../data/registers/adc_v5.yaml) | A460/F451/F452/F460: ADC2 | 35 / 33 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ll_v3_1.rs` | [adc_v2](../../data/registers/adc_v2.yaml) | A448/F448/M441: ADC2 | 39 / 48 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 10 项；缺少 AWDCHSR/AWDDR0/AWDDR1/CHMUXR2/DR8 等 6 项；定义差异 AVCHSELR/AWDCR/AWDSR/CHSELRA/CHSELRB 等 9 项 |
| `adc_ll_v3_2.rs` | [adc_v3](../../data/registers/adc_v3.yaml) | A448/F334/F336/F448/M441: ADC3 | 48 / 52 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 16 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 10 项 |
| `adc_ll_v3_3.rs` | [adc_v21](../../data/registers/adc_v21.yaml) | F467: ADC2 | 56 / 54 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 24 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 9 项 |
| `adc_ll_v3_4.rs` | [adc_v22](../../data/registers/adc_v22.yaml) | F467: ADC3 | 56 / 54 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 24 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 9 项 |
| `adc_ll_v3_5.rs` | [adc_v18](../../data/registers/adc_v18.yaml) | F334/F336: ADC2 | 57 / 56 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 25 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 10 项 |
| `adc_ll_v3_6.rs` | [adc_v1](../../data/registers/adc_v1.yaml) | A448/F334/F336/F448/M441: ADC1 | 58 / 59 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 26 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 10 项 |
| `adc_ll_v3_7.rs` | [adc_v11](../../data/registers/adc_v11.yaml) | A4A0/F4A0/F4A2: ADC3 | 61 / 54 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 29 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 9 项 |
| `adc_ll_v3_8.rs` | [adc_v14](../../data/registers/adc_v14.yaml) | A4A8/F4A8: ADC3 | 62 / 56 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 30 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 10 项 |
| `adc_ll_v3_9.rs` | [adc_v8](../../data/registers/adc_v8.yaml) | A472/F472: ADC3 | 66 / 56 | 相对 adc_ll_v3：增加 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 34 项；缺少 AWDCHSR/AWDDR0/AWDDR1；定义差异 AVCHSELR/AWDCR/AWDSR/CHMUXR2/CHSELRA 等 9 项 |

### adc：v4

逐通道采样，扩展增益/采样保持或运放控制；同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ll_v4.rs` | [adc_v19](../../data/registers/adc_v19.yaml) | F431/M413: ADC | 54 / 50 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ll_v4_1.rs` | [adc_v4](../../data/registers/adc_v4.yaml) | A460/F451/F452/F460: ADC1 | 57 / 47 | 相对 adc_ll_v4：增加 AWDCHSR/AWDDR0/AWDDR1/CHMUXR0/CHMUXR1 等 14 项；缺少 AWD0CHSR/AWD0DR0/AWD0DR1/AWD1CHSR/AWD1DR0 等 11 项；定义差异 AVCHSELR/AWDCR/AWDSR/CHSELRA/CHSELRB 等 10 项 |
| `adc_ll_v4_2.rs` | [adc_v10](../../data/registers/adc_v10.yaml) | A4A0/F4A0/F4A2: ADC2 | 59 / 57 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/EXCHSELR 等 8 项；缺少 CHSELRC/CR2/OPACR；定义差异 AWDSCLRR/CR0/CR1/ICR/ISCLRR 等 7 项 |
| `adc_ll_v4_3.rs` | [adc_v13](../../data/registers/adc_v13.yaml) | A4A8/F4A8: ADC2 | 60 / 59 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/EXCHSELR 等 8 项；缺少 CHSELRC/OPACR；定义差异 CR0/CR1/ICR/ISCLRR/ISR 等 6 项 |
| `adc_ll_v4_4.rs` | [adc_v20](../../data/registers/adc_v20.yaml) | F467: ADC1 | 61 / 64 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/EXCHSELR 等 10 项；缺少 CHSELRC/CR2/OPACR；定义差异 AWDSCLRR/CR0/CR1/ICR/ISCLRR 等 7 项 |
| `adc_ll_v4_5.rs` | [adc_v9](../../data/registers/adc_v9.yaml) | A4A0/F4A0/F4A2: ADC1 | 63 / 66 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/EXCHSELR 等 12 项；缺少 CHSELRC/CR2/OPACR；定义差异 AWDSCLRR/CR0/CR1/ICR/ISCLRR 等 7 项 |
| `adc_ll_v4_6.rs` | [adc_v12](../../data/registers/adc_v12.yaml) | A4A8/F4A8: ADC1 | 64 / 68 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/EXCHSELR 等 12 项；缺少 CHSELRC/OPACR；定义差异 CR0/CR1/ICR/ISCLRR/ISR 等 6 项 |
| `adc_ll_v4_7.rs` | [adc_v6](../../data/registers/adc_v6.yaml) | A472/F472: ADC1 | 67 / 59 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/DR16 等 16 项；缺少 CHSELRC/CR2/OPACR；定义差异 AVCHSELR/CHSELRA/CHSELRB/CR0/CR1 等 9 项 |
| `adc_ll_v4_8.rs` | [adc_v7](../../data/registers/adc_v7.yaml) | A472/F472: ADC2 | 67 / 59 | 相对 adc_ll_v4：增加 CHMUXR0/CHMUXR1/CHMUXR2/CHMUXR3/DR16 等 16 项；缺少 CHSELRC/CR2/OPACR；定义差异 AVCHSELR/CHSELRA/CHSELRB/CR0/CR1 等 9 项 |

### adc：v5

逐通道采样、SeqC/差分结果 DIR；OPACR 按实例保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `adc_ll_v5.rs` | [adc_v26](../../data/registers/adc_v26.yaml) | M458: ADC2 | 53 / 81 | 本档位对照基准；完整定义见来源 YAML |
| `adc_ll_v5_1.rs` | [adc_v25](../../data/registers/adc_v25.yaml) | M458: ADC1 | 75 / 97 | 相对 adc_ll_v5：增加 DIR10/DIR11/DIR12/DIR13/DIR14 等 23 项；缺少 ADC_EXCHSELR；定义差异 AVCHSELR/CHSELRA/CHSELRB/CHSELRC |

### cmp：v1

精简比较器，迟滞/滤波及电压选择。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmp_ll_v1.rs` | [cmp_v5](../../data/registers/cmp_v5.yaml) | A460/F451/F452/F460: CMP1, CMP2, CMP3 | 5 / 15 | 本档位对照基准；完整定义见来源 YAML |

### cmp：v2

窗口/消隐比较器；TWR/TWSR/BWSR 属不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmp_ll_v2.rs` | [cmp_v17](../../data/registers/cmp_v17.yaml) | M120: CMP1 | 6 / 28 | 本档位对照基准；完整定义见来源 YAML |
| `cmp_ll_v2_1.rs` | [cmp_v18](../../data/registers/cmp_v18.yaml) | M120: CMP2 | 6 / 29 | 相对 cmp_ll_v2：定义差异 MDR |
| `cmp_ll_v2_2.rs` | [cmp_v12](../../data/registers/cmp_v12.yaml) | A4A8/F4A8: CMP2, CMP4 | 6 / 46 | 相对 cmp_ll_v2：增加 BWPR/BWSR/PMSR；缺少 TWR1/TWR2/VSR；定义差异 MDR/OCR |
| `cmp_ll_v2_3.rs` | [cmp_v9](../../data/registers/cmp_v9.yaml) | A4A0/F467/F4A0/F4A2: CMP1, CMP3 | 7 / 46 | 相对 cmp_ll_v2：增加 PMSR/TWPR/TWSR/VISR；缺少 TWR1/TWR2/VSR |
| `cmp_ll_v2_4.rs` | [cmp_v10](../../data/registers/cmp_v10.yaml) | A4A0/F4A0/F4A2: CMP2, CMP4；F467: CMP2 | 7 / 47 | 相对 cmp_ll_v2：增加 PMSR/TWPR/TWSR/VISR；缺少 TWR1/TWR2/VSR；定义差异 MDR |
| `cmp_ll_v2_5.rs` | [cmp_v11](../../data/registers/cmp_v11.yaml) | A4A8/F4A8: CMP1, CMP3 | 7 / 47 | 相对 cmp_ll_v2：增加 BWPR/BWSR/PMSR/VISR；缺少 TWR1/TWR2/VSR；定义差异 MDR/OCR |
| `cmp_ll_v2_6.rs` | [cmp_v7](../../data/registers/cmp_v7.yaml) | A472/F472: CMP1, CMP3 | 7 / 50 | 相对 cmp_ll_v2：增加 BWSR1/BWSR2/OMR/PMSR；缺少 TWR1/TWR2/VSR；定义差异 FIR/MDR/OCR |
| `cmp_ll_v2_7.rs` | [cmp_v8](../../data/registers/cmp_v8.yaml) | A472/F472: CMP2, CMP4 | 7 / 51 | 相对 cmp_ll_v2：增加 BWSR1/BWSR2/OMR/PMSR；缺少 TWR1/TWR2/VSR；定义差异 FIR/MDR/OCR |

### cmp：v3

比较器窗口与同步控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmp_ll_v3.rs` | [cmp_v1](../../data/registers/cmp_v1.yaml) | A448/F448/M441: CMP1 | 8 / 56 | 本档位对照基准；完整定义见来源 YAML |
| `cmp_ll_v3_1.rs` | [cmp_v3](../../data/registers/cmp_v3.yaml) | A448/F448/M441: CMP3 | 8 / 56 | 相对 cmp_ll_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `cmp_ll_v3_2.rs` | [cmp_v2](../../data/registers/cmp_v2.yaml) | A448/F448/M441: CMP2 | 8 / 57 | 相对 cmp_ll_v3：定义差异 MDR |
| `cmp_ll_v3_3.rs` | [cmp_v4](../../data/registers/cmp_v4.yaml) | A448/F448/M441: CMP4 | 8 / 57 | 相对 cmp_ll_v3：定义差异 MDR |
| `cmp_ll_v3_4.rs` | [cmp_v14](../../data/registers/cmp_v14.yaml) | F431/M413: CMP1, CMP2, CMP3 | 9 / 42 | 相对 cmp_ll_v3：增加 OMR；定义差异 BWSR1/MDR/PMSR/SCCR/SCMR |
| `cmp_ll_v3_5.rs` | [cmp_v13](../../data/registers/cmp_v13.yaml) | F334/F336: CMP1, CMP2, CMP3 | 9 / 58 | 相对 cmp_ll_v3：增加 OMR；定义差异 MDR/PMSR/SCCR/SCMR |

### cmp：v4

比较器、DAC 与消隐窗口；同步控制按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmp_ll_v4.rs` | [cmp_v16](../../data/registers/cmp_v16.yaml) | K116/K118: CMP | 9 / 35 | 本档位对照基准；完整定义见来源 YAML |
| `cmp_ll_v4_1.rs` | [cmp_v19](../../data/registers/cmp_v19.yaml) | M458: CMP1, CMP2 | 11 / 62 | 相对 cmp_ll_v4：增加 SCCR/SCMR；定义差异 BWSR1/DACR/DADR/MDR/PMSR |

### cmp_common：v1

比较器共享 DAC 数据/控制窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmp_common_ll_v1.rs` | [cmp_v15](../../data/registers/cmp_v15.yaml) | F431/M413: CMP_COMMON | 2 / 4 | 本档位对照基准；完整定义见来源 YAML |
| `cmp_common_ll_v1_1.rs` | [cmp_v6](../../data/registers/cmp_v6.yaml) | A460/F451/F452/F460: CMP_COMMON | 4 / 8 | 相对 cmp_common_ll_v1：增加 DADR1/DADR2/RVADC；缺少 DADR；定义差异 DACR |

### dac：v1

双数据寄存器 DAC；对齐/增益字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dac_ll_v1.rs` | [dac_v2](../../data/registers/dac_v2.yaml) | A472/F472: DAC1, DAC2, DAC3, DAC4 | 5 / 16 | 本档位对照基准；完整定义见来源 YAML |
| `dac_ll_v1_1.rs` | [dac_v3](../../data/registers/dac_v3.yaml) | A4A0/A4A8/F4A0/F4A2/F4A8: DAC1, DAC2；F467: DAC1 | 5 / 16 | 相对 dac_ll_v1：定义差异 DACR |
| `dac_ll_v1_2.rs` | [dac_v1](../../data/registers/dac_v1.yaml) | A448/F448/M441: DAC | 5 / 48 | 相对 dac_ll_v1：定义差异 DACR/DADR1/DADR2 |

### dac：v2

DAC-ADC 硬件触发扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dac_ll_v2.rs` | [dac_v4](../../data/registers/dac_v4.yaml) | F334/F336: DAC1, DAC2 | 8 / 18 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
