# 通用、基本与低功耗定时器

目标名称恢复 tmr0、tmr2、tmr4，不沿用源数据被截断的 tmr。
TMR6 及 TMR6_COMMON 全部属于排除项，不能因共用旧 kind=tmr 而被一并编号。
TMR4_ECER 是独立附属窗口，使用 tmr4_ecer 名称。

TMR0/TMR2 按单 A、双 A/B 计数通道分档，捕获/同步/溢出位使用后缀，
不再采用“位差异可以合并加注记”的建议。M458 的 A 通道硬件触发扩展仍按实际位域保留。
TMR4 按输出/采样规模及多组 OCCR 缓冲分档；三相/四相分支不能视为彼此完全包含。
TMRA 按 2/4/6/8 路比较输出分档，计数位宽和 EPSCR/相位控制另列后缀。
此顺序优先输出通道规模，不表示八路 16 位一定比六路带相位控制更适用于所有任务。

F155 的 TMRB SVD 缺 CNTER/PERAR/CMPAR 位域，设备头掩码为 0xFFFF、结构体为 uint16_t。
A4A0/F4A0/F4A2/F467 的 TMRA 掩码为 0xFFFF；A448/F448/M441、F334/F336、A472/F472
为 0xFFFFFFFF。F431/M413 为 16 位。A472/F472 的 SVD 缺部分数据位域，此处保留来源而不补写。
F431/M413/M458 门控在 CMU.FCG，其余 M4 在 PWC.FCG2。
F175 的 GTIM0–8 与 LPTIM0/1 独立登记，不续用 DDL 的编号。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### tmr0：v1

单 A 计数通道。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr0_ll_v1.rs` | [tmr0_ll_v1](../../data/registers/tmr0_ll_v1.yaml) | F120/M120: TMR0 | 4 / 13 | 本档位对照基准；完整定义见来源 YAML |
| `tmr0_ll_v1_1.rs` | [tmr0_ll_v1_1](../../data/registers/tmr0_ll_v1_1.yaml) | F115/F155: TMR0 | 4 / 14 | 相对 tmr0_ll_v1：定义差异 BCONR |
| `tmr0_ll_v1_2.rs` | [tmr0_ll_v1_2](../../data/registers/tmr0_ll_v1_2.yaml) | K116/K118: TMR0 | 4 / 18 | 相对 tmr0_ll_v1：定义差异 BCONR/STFLR |

### tmr0：v2

双 A/B 计数通道。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr0_ll_v2.rs` | [tmr0_ll_v2](../../data/registers/tmr0_ll_v2.yaml) | M458: TMR01, TMR02 | 6 / 20 | 本档位对照基准；完整定义见来源 YAML |
| `tmr0_ll_v2_1.rs` | [tmr0_ll_v2_1](../../data/registers/tmr0_ll_v2_1.yaml) | A460/A4A0/F451/F452/F460/F467/F4A0/F4A2: TMR01, TMR02 | 6 / 28 | 相对 tmr0_ll_v2：定义差异 BCONR/STFLR |
| `tmr0_ll_v2_2.rs` | [tmr0_ll_v2_2](../../data/registers/tmr0_ll_v2_2.yaml) | A472/F472: TMR01, TMR02 | 6 / 30 | 相对 tmr0_ll_v2：定义差异 BCONR/STFLR |
| `tmr0_ll_v2_3.rs` | [tmr0_ll_v2_3](../../data/registers/tmr0_ll_v2_3.yaml) | A448/F448/M441: TMR01, TMR02 | 6 / 34 | 相对 tmr0_ll_v2：定义差异 BCONR/STFLR |
| `tmr0_ll_v2_4.rs` | [tmr0_ll_v2_4](../../data/registers/tmr0_ll_v2_4.yaml) | F431/M413: TMR01, TMR02 | 6 / 34 | 相对 tmr0_ll_v2：定义差异 BCONR/STFLR |
| `tmr0_ll_v2_5.rs` | [tmr0_ll_v2_5](../../data/registers/tmr0_ll_v2_5.yaml) | A4A8/F4A8: TMR01, TMR02, TMR03, TMR04, TMR05；F334/F336: TMR01, TMR02 | 6 / 36 | 相对 tmr0_ll_v2：定义差异 BCONR/STFLR |

### tmr2：v1

单通道计数与硬件触发。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr2_ll_v1.rs` | [tmr2_ll_v1](../../data/registers/tmr2_ll_v1.yaml) | M120: TMR2 | 7 / 30 | 本档位对照基准；完整定义见来源 YAML |

### tmr2：v2

双通道计数与硬件触发，捕获/同步位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr2_ll_v2.rs` | [tmr2_ll_v2](../../data/registers/tmr2_ll_v2.yaml) | A4A0/F4A0/F4A2: TMR21, TMR22, TMR23, TMR24 | 9 / 62 | 本档位对照基准；完整定义见来源 YAML |
| `tmr2_ll_v2_1.rs` | [tmr2_ll_v2_1](../../data/registers/tmr2_ll_v2_1.yaml) | A472/F472: TMR21, TMR22, TMR23, TMR24 | 9 / 64 | 相对 tmr2_ll_v2：定义差异 STFLR |
| `tmr2_ll_v2_2.rs` | [tmr2_ll_v2_2](../../data/registers/tmr2_ll_v2_2.yaml) | A4A8/F4A8: TMR21, TMR22, TMR23, TMR24 | 9 / 66 | 相对 tmr2_ll_v2：定义差异 STFLR |
| `tmr2_ll_v2_3.rs` | [tmr2_ll_v2_3](../../data/registers/tmr2_ll_v2_3.yaml) | K116/K118: TMR21, TMR22 | 9 / 68 | 相对 tmr2_ll_v2：定义差异 BCONR/STFLR |

### tmr4：v1

三相基础输出/采样 → 四相扩展 → 多组 OCCR 缓冲；不同相数分支不宣称包含关系。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr4_ll_v1.rs` | [tmr4_ll_v1](../../data/registers/tmr4_ll_v1.yaml) | M120: TMR4 | 54 / 249 | 本档位对照基准；完整定义见来源 YAML |
| `tmr4_ll_v1_1.rs` | [tmr4_ll_v1_1](../../data/registers/tmr4_ll_v1_1.yaml) | A460/F451/F452/F460: TMR41, TMR42, TMR43 | 54 / 249 | 相对 tmr4_ll_v1：增加 OCMRUH/OCMRUL/OCMRVH/OCMRVL/OCMRWH 等 6 项；缺少 OCMRHUH/OCMRHVH/OCMRHWH/OCMRLUL/OCMRLVL 等 6 项；定义差异 ECSR |
| `tmr4_ll_v1_2.rs` | [tmr4_ll_v1_2](../../data/registers/tmr4_ll_v1_2.yaml) | A4A0/F467/F4A0/F4A2: TMR41, TMR42, TMR43；A4A8/F4A8: TMR41 | 55 / 265 | 相对 tmr4_ll_v1：增加 OCMRUH/OCMRUL/OCMRVH/OCMRVL/OCMRWH 等 8 项；缺少 ECSR/OCMRHUH/OCMRHVH/OCMRHWH/OCMRLUL 等 7 项 |
| `tmr4_ll_v1_3.rs` | [tmr4_ll_v1_3](../../data/registers/tmr4_ll_v1_3.yaml) | A4A8/F4A8: TMR42, TMR43 | 55 / 266 | 相对 tmr4_ll_v1：增加 OCMRUH/OCMRUL/OCMRVH/OCMRVL/OCMRWH 等 8 项；缺少 ECSR/OCMRHUH/OCMRHVH/OCMRHWH/OCMRLUL 等 7 项；定义差异 CCSR |
| `tmr4_ll_v1_4.rs` | [tmr4_ll_v1_4](../../data/registers/tmr4_ll_v1_4.yaml) | A472/F472/K116/K118: TMR4 | 57 / 278 | 相对 tmr4_ll_v1：增加 OCMRUH/OCMRUL/OCMRVH/OCMRVL/OCMRWH 等 10 项；缺少 ECSR/OCMRHUH/OCMRHVH/OCMRHWH/OCMRLUL 等 7 项；定义差异 CCSR/CNTR/CPSR/OCCRUH/OCCRUL 等 15 项 |

### tmr4：v2

三相基础输出/采样 → 四相扩展 → 多组 OCCR 缓冲；不同相数分支不宣称包含关系。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr4_ll_v2.rs` | [tmr4_ll_v2](../../data/registers/tmr4_ll_v2.yaml) | A448/F448/M441: TMR41 | 73 / 364 | 本档位对照基准；完整定义见来源 YAML |
| `tmr4_ll_v2_1.rs` | [tmr4_ll_v2_1](../../data/registers/tmr4_ll_v2_1.yaml) | F334/F336: TMR4 | 73 / 364 | 相对 tmr4_ll_v2：定义差异 POCRX |
| `tmr4_ll_v2_2.rs` | [tmr4_ll_v2_2](../../data/registers/tmr4_ll_v2_2.yaml) | A448/F448/M441: TMR42 | 73 / 365 | 相对 tmr4_ll_v2：定义差异 CCSR |
| `tmr4_ll_v2_3.rs` | [tmr4_ll_v2_3](../../data/registers/tmr4_ll_v2_3.yaml) | A448/F448/M441: TMR43 | 73 / 365 | 相对 tmr4_ll_v2：定义差异 CCSR |

### tmr4：v3

三相基础输出/采样 → 四相扩展 → 多组 OCCR 缓冲；不同相数分支不宣称包含关系。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr4_ll_v3.rs` | [tmr4_ll_v3](../../data/registers/tmr4_ll_v3.yaml) | M458: TMR41 | 73 / 297 | 本档位对照基准；完整定义见来源 YAML |
| `tmr4_ll_v3_1.rs` | [tmr4_ll_v3_1](../../data/registers/tmr4_ll_v3_1.yaml) | M458: TMR42 | 73 / 298 | 相对 tmr4_ll_v3：定义差异 CCSR |
| `tmr4_ll_v3_2.rs` | [tmr4_ll_v3_2](../../data/registers/tmr4_ll_v3_2.yaml) | F431/M413: TMR4 | 94 / 390 | 相对 tmr4_ll_v3：增加 BFCRX/OCCR1XH/OCCR1XL/OCCR2XH/OCCR2XL 等 21 项；定义差异 CCSR/PSCR/RCSR/SCFR/SCIR |

### tmr4_ecer：v1

TMR4 共享外部时钟控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmr4_ecer_ll_v1.rs` | [tmr4_ecer_ll_v1](../../data/registers/tmr4_ecer_ll_v1.yaml) | A460/F451/F452/F460: TMR4_ECER | 3 / 3 | 本档位对照基准；完整定义见来源 YAML |

### tmra：v1

2 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmra_ll_v1.rs` | [tmra_ll_v1](../../data/registers/tmra_ll_v1.yaml) | M120: TMRA | 18 / 84 | 本档位对照基准；完整定义见来源 YAML |
| `tmra_ll_v1_1.rs` | [tmra_ll_v1_1](../../data/registers/tmra_ll_v1_1.yaml) | F431/M413: TMRA1, TMRA2, TMRA3, TMRA4 | 18 / 97 | 相对 tmra_ll_v1：增加 BCONR1；缺少 BCONR；定义差异 CCONR1/CCONR2/CMPAR1/CMPAR2/CNTER 等 10 项 |

### tmra：v2

4 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmra_ll_v2.rs` | [tmra_ll_v2](../../data/registers/tmra_ll_v2.yaml) | A472/F472: TMRA1 | 25 / 133 | 本档位对照基准；完整定义见来源 YAML |
| `tmra_ll_v2_1.rs` | [tmra_ll_v2_1](../../data/registers/tmra_ll_v2_1.yaml) | A472/F472: TMRA2 | 25 / 133 | 相对 tmra_ll_v2：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `tmra_ll_v2_2.rs` | [tmra_ll_v2_2](../../data/registers/tmra_ll_v2_2.yaml) | A472/F472: TMRA3 | 25 / 133 | 相对 tmra_ll_v2：定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CNTER 等 6 项 |
| `tmra_ll_v2_3.rs` | [tmra_ll_v2_3](../../data/registers/tmra_ll_v2_3.yaml) | A472/F472: TMRA4 | 25 / 133 | 相对 tmra_ll_v2：定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CNTER 等 6 项 |
| `tmra_ll_v2_4.rs` | [tmra_ll_v2_4](../../data/registers/tmra_ll_v2_4.yaml) | A472/F472: TMRA5 | 25 / 133 | 相对 tmra_ll_v2：定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CNTER 等 6 项 |
| `tmra_ll_v2_5.rs` | [tmra_ll_v2_5](../../data/registers/tmra_ll_v2_5.yaml) | A472/F472: TMRA6 | 25 / 133 | 相对 tmra_ll_v2：定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CNTER 等 6 项 |
| `tmra_ll_v2_6.rs` | [tmra_ll_v2_6](../../data/registers/tmra_ll_v2_6.yaml) | A4A0/F4A0/F4A2: TMRA1, TMRA10, TMRA11, TMRA12, TMRA2, TMRA3, TMRA4, TMRA5, TMRA6, TMRA7, TMRA8, TMRA9；F467: TMRA1, TMRA2, TMRA3, TMRA4, TMRA5, TMRA6, TMRA7, TMRA8 | 25 / 135 | 相对 tmra_ll_v2：定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CNTER 等 7 项 |
| `tmra_ll_v2_7.rs` | [tmra_ll_v2_7](../../data/registers/tmra_ll_v2_7.yaml) | A4A8/F4A8: TMRA1, TMRA10, TMRA11, TMRA12, TMRA2, TMRA3, TMRA4, TMRA5, TMRA6, TMRA7, TMRA8, TMRA9 | 25 / 143 | 相对 tmra_ll_v2：定义差异 BCONR1/BCONR2/CCONR1/CCONR2/CCONR3 等 12 项 |
| `tmra_ll_v2_8.rs` | [tmra_ll_v2_8](../../data/registers/tmra_ll_v2_8.yaml) | F334/F336: TMRA1 | 25 / 143 | 相对 tmra_ll_v2：定义差异 BCONR1/BCONR2/CCONR1/CCONR2/CCONR3 等 6 项 |
| `tmra_ll_v2_9.rs` | [tmra_ll_v2_9](../../data/registers/tmra_ll_v2_9.yaml) | F334/F336: TMRA2, TMRA3, TMRA4, TMRA5 | 25 / 143 | 相对 tmra_ll_v2：定义差异 BCONR1/BCONR2/CCONR1/CCONR2/CCONR3 等 12 项 |

### tmra：v3

6 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmra_ll_v3.rs` | [tmra_ll_v3](../../data/registers/tmra_ll_v3.yaml) | F431/M413: TMRA5 | 33 / 193 | 本档位对照基准；完整定义见来源 YAML |
| `tmra_ll_v3_1.rs` | [tmra_ll_v3_1](../../data/registers/tmra_ll_v3_1.yaml) | M458: TMRA1, TMRA2, TMRA3, TMRA4, TMRA5 | 36 / 205 | 相对 tmra_ll_v3：增加 PBCONR/PHSCR/SPECONR；定义差异 CMPAR1/CMPAR2/CMPAR3/CMPAR4/CMPAR5 等 9 项 |

### tmra：v4

8 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmra_ll_v4.rs` | [tmra_ll_v4](../../data/registers/tmra_ll_v4.yaml) | A460/F451/F452/F460: TMRA1, TMRA2, TMRA3, TMRA4, TMRA5, TMRA6 | 39 / 213 | 本档位对照基准；完整定义见来源 YAML |
| `tmra_ll_v4_1.rs` | [tmra_ll_v4_1](../../data/registers/tmra_ll_v4_1.yaml) | K116/K118: TMRA | 39 / 220 | 相对 tmra_ll_v4：定义差异 BCSTRL/CCONR1/CCONR2/CCONR3/CCONR4 等 23 项 |
| `tmra_ll_v4_2.rs` | [tmra_ll_v4_2](../../data/registers/tmra_ll_v4_2.yaml) | A448/F448/M441: TMRA1, TMRA2, TMRA3, TMRA4, TMRA5 | 39 / 241 | 相对 tmra_ll_v4：定义差异 BCONR1/BCONR2/BCONR3/BCONR4/CCONR1 等 23 项 |

### tmrb：v1

基本计数与比较，缺失位域按来源保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `tmrb_ll_v1.rs` | [tmrb_ll_v1](../../data/registers/tmrb_ll_v1.yaml) | F155: TMRB1, TMRB2, TMRB3, TMRB4, TMRB5, TMRB6, TMRB7, TMRB8 | 13 / 46 | 本档位对照基准；完整定义见来源 YAML |
| `tmrb_ll_v1_1.rs` | [tmrb_ll_v1_1](../../data/registers/tmrb_ll_v1_1.yaml) | F115/F120: TMRB1, TMRB2, TMRB3, TMRB4, TMRB5, TMRB6, TMRB7, TMRB8；M120: TMRB1, TMRB2, TMRB3, TMRB4 | 13 / 49 | 相对 tmrb_ll_v1：定义差异 CMPAR/CNTER/PERAR |
<!-- peripheral-versions:end -->
