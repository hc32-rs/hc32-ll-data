# ADC、DAC 与比较器

## Cortex-M0+

| 分类 | 系列 | 要点 |
|---|---|---|
| `adc_v5a` | F175 | 旧式序列布局，26 个顶层寄存器，设备头 ADC 基址 0x40007800 |
| `adc_v9` | F115/F120/F155、K116/K118、M120 | 新式 SeqA/SeqB 共享分类候选。通道及寄存器增量分系列记录 |
| `cmp_v1` | K116/K118 | 带 DAC 和消隐窗 |
| `cmp_v2` | M120 | 窗口模式 |

F155、K116、K118 和 M120 的 SDK 各有 `projects/*/examples/adc/adc_dma/source/main.c`。
例程用 `AOS_SetTriggerEventSrc` 将 `EVT_SRC_ADC_EOCA` 路由到 DMA 通道。
F115/F120 无 DMA。F175 的 DMAC 使用自己的触发选择字段，ADC 请求见
`hc32_ll_dmac.h` 的 `DMA_ADC_JRQ_DREQ` 和 `DMA_ADC_EOC_DREQ` 定义。

## Cortex-M4

ADC 统一归入 `adc_v10` 家族，按实例记录 `CR2/AVCHSELR/SSTRL/SYNCCR/SHCR/PGA/DIR/CHSELRC/OPACR`
等可选块以及采样时间、结果寄存器数量。SVD 按寄存器名、偏移、位域的静态对比得到 19 个签名，
其中 A472/F472 的 ADC1/2 同签名，F467 的 ADC2/3 同签名。
A448/F448/M441 的 ADC3 与 F334/F336 的 ADC3 同签名。
这些签名未覆盖访问语义和复位值，合并前仍须核对设备头。

| version | 系列 | 差异（字段级） |
|---|---|---|
| `dac_v3` | A4A0 A4A8 F467 F4A0 F4A2 F4A8 | 基准版；DADR 为无字段纯数据寄存器 |
| `dac_v3_1` | A448 F448 M441 | DADR1/2 带 DL0R4/DL1R5…/DR0..3 对齐与双数据字段 |
| `dac_v3_2` | A472 F472 | DACR 带 DAOGS1/2 增益位（无 EXTDSL） |
| `dac_v3_3` | F334 F336 | 多 `DACR2 DADACTR1 DADACTR2`（DAC-ADC 硬件触发控制，8 寄存器） |

DAC 实例数为 A472/F472 各 4 个，A448/F448/M441 各 1 个，其余表列系列各 2 个。

| 版本 | 系列 | 寄存器 | 特征 |
|---|---|---|---|
| `cmp_v3` | M458 | MDR FIR OCR OMR PMSR DADR DACR BWSR1 BWSR2 SCCR SCMR（11） | 与 K116 cmp_v1 同族（多 SCCR/SCMR；DACR 字段名 DAEN/DA2POE/REFSEL ≠ K116 的 DAE） |
| `cmp_v4` | A448 F448 M441 | MDR FIR OCR PMSR BWSR1 BWSR2 SCCR SCMR（8） | 无 DAC/OMR；MDR 带 CMON/CSMD/CSST；BWSR1 16 位窗 |
| `cmp_v4_1` | F431 M413 | 同上 8+OMR=9 | BWSR1 仅 8 位窗；MDR 多 HYST/WDEN |
| `cmp_v4_2` | F334 F336 | 9（同名 F431 簇） | 字段级差异另成簇（保守拆版） |
| `cmp_v5` | A472 F472 | MDR FIR OCR OMR PMSR BWSR1 BWSR2（7） | 无 SCCR/SCMR/DAC |
| `cmp_v6` | A4A0 A4A8 F467 F4A0 F4A2 F4A8 | MDR FIR OCR PMSR TWSR TWPR [VISR]（6/7） | **窗口模式**（`TWSR/TWPR/VISR`）；CMP1/3 有 VISR、CMP2/4 无；A4A8/F4A8 用 BWSR/BWPR 代替 TWSR/TWPR（严格另版 `cmp_v6_1`）+ MDR HYST/CENA |
| `cmp_v7` | A460 F451 F452 F460 | CTRL VLTSEL OUTMON CVSSTB CVSPRD（5） | **mini CMP**（迟滞/滤波/电压选择/稳定周期），与 MDR 系完全不同的精简 IP，输出可做 AOS 事件 |

F431/M413/M458 的 ADC/CMP 门控在 `CMU.FCG`（驱动宏 `FCG0_PERIPH_*`），其他 M4 LL 系列在
`PWC.FCG3`。A448/F448/M441 的设备头提供具名 ADC/CMP 向量，例如 A448 的
`ADC1_IRQn=128`、`CMP1_IRQn=89`。中断和 AOS 事件分别读取 `IRQn_Type`、`en_event_src_t`。
