# 时钟、复位与校准

## Cortex-M0+

| 版本 | 系列 |
|---|---|
| `sysctrl_v13`、`ctrim_v5`、`csu_v1` | F175 |
| `cmu_v1/v2/v3/v4/v5` | 分别为 K116/K118、F115、F155、F120、M120 |
| `reset_v9/v10` | v9 为 F115/F120/F155/M120；v10 为 K116/K118 |
| `ctc_v1/v2/v3` | 分别为 F115/F120/M120、K116/K118、F155 |
| `icg_v1/v2/v3/v4` | 分别为 F115、F120、F155、M120 |
| `icg_v5/v6` | K116/K118 的 ICGA、ICGB 两个窗口 |

F175 的 `hc32f175.h` 引用 `core_cm0plus.h`。时钟块为 SYSCTRL（0x40025000），驱动门控宏映射到
`SYSCTRL_APBEN0/1` 和 `SYSCTRL_AHBEN0`。其余六个 Cortex-M0+ 系列使用 `CMU.FCG`。

## Cortex-M4

| 分类建议 | 范围 |
|---|---|
| `ctc_v1` | F467/F4A0/F4A2/A4A0，SVD 布局及所查设备头位掩码与 M0+ v1 一致 |
| `ctc_v3` | A448/F448/M441、F334/F336，SVD 布局及所查设备头位掩码与 F155 一致 |
| `ctc_v4` | A472/F472 |
| `ctc_v1` 子型 | F4A8/A4A8 的 SVD 三寄存器签名与 v1 相同，设备头宏集合较少，需按系列核对可用位 |

| CMU 版本 | 系列 | 布局 |
|---|---|---|
| `cmu_v6` | F451 F452 F460 A460 | 连续偏移（0x20-0x104），PLL+UPLL+USB |
| `cmu_v7` | F467 F4A0 F4A2 A4A0 | 稀疏偏移（0x7Cxx/0x40004xx 窗口），PLLH+PLLA |
| `cmu_v8` | F431 M413 | 类 M0 紧凑布局 + PLLCFGR/PLLCR@0x44/48，**保留 FCG@0x30** |
| `cmu_v9` | M458 | 骨架同 cmu_v8，字段不同；**保留 FCG@0x30** |
| `cmu_v10` | F334 F336 | 稀疏偏移 + XTALDIVR/XTALDIVCR |
| `cmu_v11` | A448 F448 | 骨架同 cmu_v10（9 寄存器同名同偏移），字段不同 |
| `cmu_v12` | M441 | 骨架同 cmu_v10/11，字段不同 |
| `cmu_v13` | F472 A472 | 骨架同 cmu_v10/11/12 + USBCKCFGR，字段不同 |
| `cmu_v14` | F4A8 A4A8 | 稀疏偏移 + PLLH+PLLA + RTCLRCTRM |

| RMU 版本 | 系列 | 寄存器骨架 |
|---|---|---|
| `reset_v11` | F451 F452 F460 A460 | RSTF0@0x0（单寄存器，骨架同 M0 RMU） |
| `reset_v12` | F334 F336 | FRST0-3@0-0xC, PRSTCR0@0x10, RSTF0@0x14 |
| `reset_v13` | A448 F448 | 同上骨架 |
| `reset_v14` | M441 | 同上骨架 |
| `reset_v15` | F472 A472 | 同上骨架 |
| `reset_v16` | F467 F4A0 F4A2 A4A0 | PRSTCR0@0xF8, RSTF0@0xFC |
| `reset_v17` | F431 M413 | RSTF0@0x0, FRST0@0x4 |
| `reset_v18` | F4A8 A4A8 | FRST0-3@0xC0-0xCC, PRSTCR0@0xF8, RSTF0@0xFC |
| `reset_v19` | M458 | RSTF0@0x0, FRST0-3@0x10-0x1C |

| ICG 版本 | 系列 | 寄存器骨架 |
|---|---|---|
| `icg_v7` | F334 F336 | ICG0/1/3@0/4/C |
| `icg_v8` | A448 F448 M441 F467 F4A0 F4A2 A4A0 | ICG0/1/3 |
| `icg_v9` | F472 A472 | ICG0/1/3/4 |
| `icg_v10` | F4A8 A4A8 | ICG0/1/3 + ICG8A/8B/8C@0x60-0x68 |
| `icg_v11` | F451 F452 F460 A460 | ICG0-7（骨架同 M0 M120 icg_v4，**字段不同不并入**） |
| `icg_v12` | F431 M413 | ICG0-4 |
| `icg_v13` | M458 | ICG0/1/2/4 |

FCM 的归档 SVD 可分成四个寄存器布局组。保留 K116/K118 的 `fcm_v1` 编号，M4 的其他编号
需按 SVD 和设备头一起复核。F431/M413/M458 与 K116/K118 同属九寄存器 SVD 组，
其中 F431/M413 的设备头掩码与 K116/K118 不完全一致。另一九寄存器组含 F334/F336、
A448/F448/M441、A460/F451/F452/F460、A472/F472、F467/F4A0/F4A2/A4A0；
组内设备头宏集合也有差异。A4A8/F4A8 的 FCM1–4 与 FCM_COM 分开建模，共享状态窗口
FCM_COM 与 FCM1 同基址。

逐套核对 `drivers/hc32_ll_driver/inc/hc32_ll_fcg.h` 后，F431/M413/M458 的门控在
`CMU.FCG`，其他 17 个 M4 系列在 `PWC.FCG0–3`。向量分组见[中断表](gpio-int.md)。
