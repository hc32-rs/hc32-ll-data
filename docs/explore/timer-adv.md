# 高级定时器与紧急刹车

F175 ATIM3@0x4000E400 是一个完整寄存器块，由 hc32_ll_atim3.h 驱动，
含 MODE0/MODE1/MODE23 控制及多通道寄存器；原文“单寄存器块”明确为“单个块”，并非一个寄存器。
M4 主定时器在[通用定时器](timer-gen.md)按 TMR4/TMRA 登记。

EMB 从基础刹车到 RLSSEL 释放选择，再到 EDGESEL 边沿选择分档。
同系列不同实例的使能源也有差异，按 SVD 和 hc32_ll_emb.h 保留后缀。
A472/F472 设备头只定义 CM_EMB0..6，SVD 的 EMB_CTL1 基址为零且重复 CTL2 至 RLSSEL。
现有生成链仍包含此节点，因此本次用 emb_ctl1 独立保留审计映射，并标明不是有效硬件实例确认。
此命名整理不修补 SVD；若后续修复，应先登记 SVD CHANGELOG 并重跑受影响流水线。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### emb：v1

基础刹车 → 释放选择 → 边沿选择；各实例使能源分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `emb_ll_v1.rs` | [emb_ll_v1](../../data/registers/emb_ll_v1.yaml) | M120: EMB | 6 / 28 | 本档位对照基准；完整定义见来源 YAML |
| `emb_ll_v1_1.rs` | [emb_ll_v1_1](../../data/registers/emb_ll_v1_1.yaml) | A460/F451/F452/F460: EMB0, EMB1, EMB2, EMB3 | 6 / 29 | 相对 emb_ll_v1：定义差异 CTL |
| `emb_ll_v1_2.rs` | [emb_ll_v1_2](../../data/registers/emb_ll_v1_2.yaml) | A448/F448/M441: EMB0 | 7 / 66 | 相对 emb_ll_v1：增加 EMB_CTL1/EMB_CTL2/EMB_INTEN/EMB_RLSSEL/EMB_SOE 等 7 项；缺少 CTL/INTEN/PWMLV/SOE/STAT 等 6 项 |

### emb：v2

基础刹车 → 释放选择 → 边沿选择；各实例使能源分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `emb_ll_v2.rs` | [emb_ll_v2](../../data/registers/emb_ll_v2.yaml) | F431/M413: EMB0, EMB1 | 7 / 55 | 本档位对照基准；完整定义见来源 YAML |
| `emb_ll_v2_1.rs` | [emb_ll_v2_1](../../data/registers/emb_ll_v2_1.yaml) | F467: EMB0, EMB1, EMB2, EMB3 | 7 / 62 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_2.rs` | [emb_ll_v2_2](../../data/registers/emb_ll_v2_2.yaml) | F467: EMB4, EMB5, EMB6 | 7 / 62 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_3.rs` | [emb_ll_v2_3](../../data/registers/emb_ll_v2_3.yaml) | A4A0/F4A0/F4A2: EMB4, EMB5, EMB6 | 7 / 63 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_4.rs` | [emb_ll_v2_4](../../data/registers/emb_ll_v2_4.yaml) | K116/K118: EMB | 7 / 66 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_5.rs` | [emb_ll_v2_5](../../data/registers/emb_ll_v2_5.yaml) | A4A8/F4A8: EMB4, EMB5, EMB6 | 7 / 69 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_6.rs` | [emb_ll_v2_6](../../data/registers/emb_ll_v2_6.yaml) | A448/F448/M441: EMB1, EMB2, EMB3 | 7 / 70 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_7.rs` | [emb_ll_v2_7](../../data/registers/emb_ll_v2_7.yaml) | A4A0/F4A0/F4A2: EMB0, EMB1, EMB2, EMB3 | 7 / 73 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_8.rs` | [emb_ll_v2_8](../../data/registers/emb_ll_v2_8.yaml) | A4A8/F4A8: EMB0, EMB1, EMB2, EMB3 | 7 / 79 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_9.rs` | [emb_ll_v2_9](../../data/registers/emb_ll_v2_9.yaml) | A472/F472: EMB0, EMB1, EMB2, EMB3, EMB4, EMB5, EMB6 | 7 / 91 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_10.rs` | [emb_ll_v2_10](../../data/registers/emb_ll_v2_10.yaml) | F334/F336: EMB6, EMB7, EMB8 | 7 / 100 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |
| `emb_ll_v2_11.rs` | [emb_ll_v2_11](../../data/registers/emb_ll_v2_11.yaml) | F334/F336: EMB0, EMB1, EMB2, EMB3, EMB4, EMB5 | 7 / 104 | 相对 emb_ll_v2：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |

### emb：v3

基础刹车 → 释放选择 → 边沿选择；各实例使能源分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `emb_ll_v3.rs` | [emb_ll_v3](../../data/registers/emb_ll_v3.yaml) | M458: EMB2, EMB3 | 8 / 60 | 本档位对照基准；完整定义见来源 YAML |
| `emb_ll_v3_1.rs` | [emb_ll_v3_1](../../data/registers/emb_ll_v3_1.yaml) | M458: EMB0, EMB1 | 8 / 71 | 相对 emb_ll_v3：定义差异 CTL1/CTL2/INTEN/RLSSEL/STAT 等 6 项 |

### emb_ctl1：v1

SVD 的零基址附属节点，保留审计映射，不作为可用硬件实例背书。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `emb_ctl1_ll_v1.rs` | [emb_ctl1_ll_v1](../../data/registers/emb_ctl1_ll_v1.yaml) | A472/F472: EMB_CTL1 | 6 / 54 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
