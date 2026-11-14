# 通用、基本与低功耗定时器

## Cortex-M0+

| 分类 | 系列 |
|---|---|
| `gtim_v2`、`lptim_v5` | F175 的 GTIM0–8、LPTIM0/1 |
| `tmr0_v1/v2/v3` | 分别为 F115/F155、F120/M120、K116/K118 |
| `tmrb_v1` | F115/F120/F155/M120 |
| `tmr2_v1/v2`、`tmr4_v1/v2`、`tmra_v1/v2` | 分别为 K116/K118、M120 |

F155 的 TMRB SVD 在 CNTER/PERAR/CMPAR 处缺少位域，设备头三者掩码均为 0xFFFF，
结构体为 `uint16_t` 寄存器。

## Cortex-M4

| 版本 | 系列 | 主要差异 |
|---|---|---|
| tmr0_v4（续） | A460/A4A0/F451/F452/F460/F467/F4A0/F4A2（16 实例） | STFLR=CMFA/CMFB、BCONR 用 INTENA/INTENB（延续 M0 v1 F115 命名） |
| tmr0_v5（续） | A4A8/F334/F336/F4A8（14 实例） | STFLR=ICPA/ICPB/OVFB/SYDB、CMENB/OVENB（延续 M0 v3 K116 命名） |
| tmr0_v6（续） | A448/F448/M441（6 实例） | 同 v5 但 STFLR 无 ICPB/OVFB/SYDB、有 SYDA |
| tmr0_v7（续） | A472/F472（4 实例） | 同 v6，ICPA 位序不同（@2 对 @1）+ OVFA/SYDA |
| tmr0_v8（续） | F431/M413（4 实例） | BCONR 有 ASYNCLKA、无 B 通道时钟选择位 |
| tmr0_v9（续） | M458（2 实例） | A 通道增强：CAPMDA/HCLEA/HICPA/HSTAA/HSTPA/SYNCLKA，STFLR=ICPA/OVFA |

建议：v5/v6/v7 差异仅 STFLR 个位，可合并为单版 + 注记；v4、v8、v9 独立。

| 版本 | 系列 | 主要差异 |
|---|---|---|
| tmr2_v3（续） | A4A0/F4A0/F4A2（12 实例） | 增 CMPASTP/CMPBSTP + STFLR 增 ICPA/ICPB/SYDA/SYDB |
| tmr2_v4（续） | A472/F472（8 实例） | 同上但 STFLR 无 ICPA/ICPB |
| tmr2_v5（续） | A4A8/F4A8（8 实例） | 仅 CMPASTP/CMPBSTP + SYNCLKAT/BT 收窄 |

建议：三簇可合并为 **tmr2_v3**（+ 注记 STFLR 位差异）。

TMR4 的 57 寄存器签名在 A472/F472 与 K116/K118 一致，可沿用 `tmr4_v1`。
其他系列按寄存器布局归 `tmr4_v3–v7` 候选，A4A8/F4A8 有额外的 TMR4 实例位域变体。
A460/F451/F452/F460 还有 0x40055408 的 TMR4_ECER 附属块。

TMRA 候选编号为 `tmra_v3–v10`，依次覆盖 A4A0/F4A0/F4A2/F467、A460/F451/F452/F460、
A448/F448/M441、A472/F472、A4A8/F4A8 与 F334/F336、F431/M413 的 TMRA1–4、
F431/M413 的 TMRA5、M458。按设备头复查 CNTER/PERAR/CMPAR 宽度：
A4A0/F4A0/F4A2/F467 的掩码为 0xFFFF，A448/F448/M441、F334/F336、A472/F472
为 0xFFFFFFFF。F431/M413 的 16 位数据由结构体和设备头掩码确认。
A472/F472 的 SVD 缺部分数据字段，设备头可作为补录依据。

F431/M413/M458 通过 `CMU.FCG` 控制定时器门控，其他 M4 系列通过 `PWC.FCG2`。
M458 包含 `hc32_ll_aos.h`，该驱动列出 DMA 和 ADC 等目标，目标选择见驱动的 `AOS_*` 定义。
