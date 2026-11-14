# 高级定时器与紧急刹车

F175 的 ATIM3（0x4000E400）由 `hc32_ll_atim3.h` 驱动，设备头和 SVD 均表达为单寄存器块。
其与旧式多模式布局的差异适合记录为独立的 `atim_v3f` 候选。
LL 的 Cortex-M4 高级定时器主体按 [通用定时器页](timer-gen.md)的 TMR4/TMRA 分类，
与其配套的紧急刹车（EMB）分组如下。

| 版本 | 系列 |
|---|---|
| `emb_v1` | M120 |
| `emb_v2` | K116/K118 |
| `emb_v3` | A448/F448/M441 |
| `emb_v4` | A460/F451/F452/F460 |
| `emb_v5` | A472/F472 |
| `emb_v6` | A4A0/F4A0/F4A2 |
| `emb_v7` | A4A8/F4A8 |
| `emb_v8` | F334/F336 |
| `emb_v9` | F431/M413 |
| `emb_v10` | F467 |
| `emb_v11` | M458 |

同系列的 EMB 实例也可能有独立字段集，按 SVD 各实例和 `hc32_ll_emb.h` 的使能源建模。
A472/F472 的设备头只定义 `CM_EMB0..6`，SVD 的 `EMB_CTL1` 节点基址为 0，节点内重复 CTL2 至 RLSSEL 的偏移。
实例按 EMB0–6 与设备头的 `CM_EMB_TypeDef` 建模。
