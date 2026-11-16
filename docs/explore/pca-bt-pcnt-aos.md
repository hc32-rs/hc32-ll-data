# 异步事件路由 AOS

F175 无 AOS；其余 26 个 LL 系列有 AOS。LL 的 SVD 无独立 PCA、PCNT 或 EAU。
AOS 按路由能力与目标规模分档：基础定时器/ADC、事件端口、扩展模拟/定时器目标，再到大型 M4 矩阵。
F115/F120 的五个寄存器偏移和位区间相同，但命名不同；F155 增加两个 DMA 目标，保留同档后缀。
两个 74 寄存器组的目标阵容不同，用数字后缀区分。M458 有两个 DMA 通道和六个 ADC 目标。
后续 87/94 寄存器矩阵表示更大路由规模，并不意味着包含前一组全部目标。

M0+ 基址为 0x40000C00，M4 为 0x40010800。
事件号取设备头 EVT_SRC_*，目标选择取 hc32_ll_aos.h。
F431/M413/M458 的 FCG0_PERIPH_AOS 映射 CMU_FCG_AOS，其余 M4 映射 PWC_FCG0_AOS。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### aos：v1

基础定时器/ADC 路由，DMA 扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v1.rs` | [aos_v6](../../data/registers/aos_v6.yaml) | F115: AOS | 5 / 5 | 本档位对照基准；完整定义见来源 YAML |
| `aos_ll_v1_1.rs` | [aos_v7](../../data/registers/aos_v7.yaml) | F120: AOS | 5 / 5 | 相对 aos_ll_v1：增加 ADC_ITRGSELR0/ADC_ITRGSELR1/INTC_STRGCR/TMR0_HTSSR/TMRB_HTSSR；缺少 ADC_TRGSEL0/ADC_TRGSEL1/INTC_SFTTRG/TMR0_TRGSEL/TMRB_TRGSEL |
| `aos_ll_v1_2.rs` | [aos_v8](../../data/registers/aos_v8.yaml) | F155: AOS | 7 / 7 | 相对 aos_ll_v1：增加 DMA_TRGSEL0/DMA_TRGSEL1/INTC_STRGCR；缺少 INTC_SFTTRG |

### aos：v2

增加事件端口与定时器目标。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v2.rs` | [aos_v12](../../data/registers/aos_v12.yaml) | M120: AOS | 11 / 23 | 本档位对照基准；完整定义见来源 YAML |

### aos：v3

六个 ADC 目标与 DMA/公共触发。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v3.rs` | [aos_v13](../../data/registers/aos_v13.yaml) | M458: AOS | 12 / 21 | 本档位对照基准；完整定义见来源 YAML |

### aos：v4

增加 DCU/TMRA/TMR4 等路由目标。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v4.rs` | [aos_v10](../../data/registers/aos_v10.yaml) | F431/M413: AOS | 16 / 29 | 本档位对照基准；完整定义见来源 YAML |

### aos：v5

多定时器、DMA 与事件端口路由。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v5.rs` | [aos_v11](../../data/registers/aos_v11.yaml) | K116/K118: AOS | 46 / 69 | 本档位对照基准；完整定义见来源 YAML |

### aos：v6

M4 路由矩阵基础。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v6.rs` | [aos_v2](../../data/registers/aos_v2.yaml) | A460/F451/F452/F460: AOS | 57 / 89 | 本档位对照基准；完整定义见来源 YAML |

### aos：v7

扩展定时器/模拟路由目标。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v7.rs` | [aos_v9](../../data/registers/aos_v9.yaml) | F334/F336: AOS | 63 / 101 | 本档位对照基准；完整定义见来源 YAML |

### aos：v8

两组 74 寄存器路由，目标阵容不同。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v8.rs` | [aos_v3](../../data/registers/aos_v3.yaml) | A472/F472: AOS | 74 / 123 | 本档位对照基准；完整定义见来源 YAML |
| `aos_ll_v8_1.rs` | [aos_v4](../../data/registers/aos_v4.yaml) | A4A0/F467/F4A0/F4A2: AOS | 74 / 123 | 相对 aos_ll_v8：增加 COMTRG1/COMTRG2/DCU_TRGSEL4；缺少 COMTRGSEL1/COMTRGSEL2/TMR4_TRGSEL；定义差异 DMA1_TRGSEL0/DMA1_TRGSEL1/DMA1_TRGSEL2/DMA1_TRGSEL3/DMA1_TRGSEL4 等 21 项 |

### aos：v9

87 寄存器路由矩阵。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v9.rs` | [aos_v1](../../data/registers/aos_v1.yaml) | A448/F448/M441: AOS | 87 / 180 | 本档位对照基准；完整定义见来源 YAML |

### aos：v10

94 寄存器路由矩阵。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aos_ll_v10.rs` | [aos_v5](../../data/registers/aos_v5.yaml) | A4A8/F4A8: AOS | 94 / 201 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
