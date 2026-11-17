# DMA

F175 的 DMAC 为 CONF/CONFA/CONFB/SRCADR/DSTADR 旧架构，使用独立 dmac 名称。
DMA 则分基本传输与逐通道 LLP、重复/非连续传输两档。BASE1_LLP/BASE2_LLP、CHENCLR、
HPROT 与通道规模使用后缀保留，不再沿用 DDL 的主编号。

F115/F120 无 DMA。F175 为一组四通道，DMAC_IRQn=7，基址 0x40024800。
M120/F155 为一组两通道，K116/K118 为一组四通道，基址 0x40013000。
M120 无 BASE1_LLP/BASE2_LLP，F155/K116/K118 增加链表选择。
M4 基址 0x40053000，第二控制器 0x40053400，通道步长 0x40，触发经 AOS。
A460/F451/F452/F460 为两组四通道且无 CHENCLR，其余 M4 的 CHENCLR/HPROT 依实例登记。
F460 的 DMA_SWREQ_SWREQ 在设备头与 SVD 都为 8 位掩码 0xFF，不能按四个通道裁剪。

F155/M120 通过 INTC 签入通用槽，K116/K118 使用 DMA_IRQn=4。
M4 向量采用[设备头向量分组](gpio-int.md)，不从布局版本推断 IRQ。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### dma：v1

基本传输；BASE1_LLP/BASE2_LLP 链表扩展分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dma_ll_v1.rs` | [dma_ll_v1](../../data/registers/dma_ll_v1.yaml) | M120: DMA | 18 / 41 | 本档位对照基准；完整定义见来源 YAML |
| `dma_ll_v1_1.rs` | [dma_ll_v1_1](../../data/registers/dma_ll_v1_1.yaml) | F155: DMA | 20 / 45 | 相对 dma_ll_v1：增加 BASE1_LLP/BASE2_LLP；定义差异 CH0CTL1/CH1CTL1/CHENCLR |
| `dma_ll_v1_2.rs` | [dma_ll_v1_2](../../data/registers/dma_ll_v1_2.yaml) | K116/K118: DMA | 28 / 71 | 相对 dma_ll_v1：增加 BASE1_LLP/BASE2_LLP/CH2CTL0/CH2CTL1/CH3CTL0 等 10 项；定义差异 CH0CTL1/CH1CTL1/CHEN/CHENCLR/CHSTAT 等 11 项 |

### dma：v2

逐通道 LLP 与重复/非连续传输；CHENCLR/HPROT、通道数分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dma_ll_v2.rs` | [dma_ll_v2](../../data/registers/dma_ll_v2.yaml) | A460/F451/F452/F460: DMA1, DMA2 | 80 / 161 | 本档位对照基准；完整定义见来源 YAML |
| `dma_ll_v2_1.rs` | [dma_ll_v2_1](../../data/registers/dma_ll_v2_1.yaml) | M458: DMA | 47 / 96 | 相对 dma_ll_v2：增加 CHENCLR；缺少 CHCTL2/CHCTL3/DAR2/DAR3/DNSEQCTL2 等 34 项；定义差异 CHEN/CHSTAT/INTCLR0/INTCLR1/INTMASK0 等 10 项 |
| `dma_ll_v2_2.rs` | [dma_ll_v2_2](../../data/registers/dma_ll_v2_2.yaml) | F431/M413: DMA | 81 / 162 | 相对 dma_ll_v2：增加 CHENCLR；定义差异 INTCLR0/INTCLR1/SWREQ |
| `dma_ll_v2_3.rs` | [dma_ll_v2_3](../../data/registers/dma_ll_v2_3.yaml) | M441: DMA1, DMA2 | 115 / 228 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHENCLR/DAR4/DAR5 等 35 项；定义差异 CHEN/CHSTAT/INTCLR0/INTCLR1/INTMASK0 等 9 项 |
| `dma_ll_v2_4.rs` | [dma_ll_v2_4](../../data/registers/dma_ll_v2_4.yaml) | F334/F336: DMA | 149 / 294 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHCTL6/CHCTL7/CHENCLR 等 69 项；定义差异 CHEN/CHSTAT/INTCLR0/INTCLR1/INTMASK0 等 9 项 |
| `dma_ll_v2_5.rs` | [dma_ll_v2_5](../../data/registers/dma_ll_v2_5.yaml) | A448/F448: DMA1, DMA2 | 115 / 234 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHENCLR/DAR4/DAR5 等 35 项；定义差异 CHCTL0/CHCTL1/CHCTL2/CHCTL3/CHEN 等 13 项 |
| `dma_ll_v2_6.rs` | [dma_ll_v2_6](../../data/registers/dma_ll_v2_6.yaml) | A472/F472: DMA1, DMA2 | 149 / 302 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHCTL6/CHCTL7/CHENCLR 等 69 项；定义差异 CHCTL0/CHCTL1/CHCTL2/CHCTL3/CHEN 等 14 项 |
| `dma_ll_v2_7.rs` | [dma_ll_v2_7](../../data/registers/dma_ll_v2_7.yaml) | A4A0/F467/F4A0/F4A2: DMA1, DMA2 | 149 / 302 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHCTL6/CHCTL7/CHENCLR 等 69 项；定义差异 CHCTL0/CHCTL1/CHCTL2/CHCTL3/CHEN 等 13 项 |
| `dma_ll_v2_8.rs` | [dma_ll_v2_8](../../data/registers/dma_ll_v2_8.yaml) | A4A8/F4A8: DMA1, DMA2 | 149 / 302 | 相对 dma_ll_v2：增加 CHCTL4/CHCTL5/CHCTL6/CHCTL7/CHENCLR 等 69 项；定义差异 CHCTL0/CHCTL1/CHCTL2/CHCTL3/CHEN 等 13 项 |

### dmac：v1

旧式 CONF/CONFA DMA；4 个源地址窗口，触发位与字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `dmac_ll_v1.rs` | [dmac_ll_v1](../../data/registers/dmac_ll_v1.yaml) | F175: DMAC | 17 / 79 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
