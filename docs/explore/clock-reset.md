# 时钟、复位与校准

CMU 按无 PLL、单 PLL、PLLH 扩展输出与双 PLL 分档；PLL+UPLL 与 PLLH+PLLA 是不同分支。
XTAL32、CAN/USB/音频时钟、XTALDIV 和 RTCLRCTRM 按实例保留。
F175 使用 SYSCTRL@0x40025000，宏映射 SYSCTRL_APBEN0/1、SYSCTRL_AHBEN0，设备头引用 core_cm0plus.h。
F431/M413/M458 和另外六个 M0+ 系列的门控在 CMU.FCG，其他 17 个 M4 在 PWC.FCG0–3。

RMU 使用实际外设名 rmu，从仅状态到外设复位分组分档，不再沿用跨库 reset 编号。
ICG/ICGA/ICGB 保留独立窗口；配置目标阵容不同，不用寄存器数推断功能包含关系。
PERIC 是独立系统控制窗口，即使包含被排除外设的使能位，本次仍按完整现有布局登记，不拆除其字段。

CTC 的三寄存器布局与设备头宏集合不总是一致，尤其 A4A8/F4A8 仍需按系列确认可用位。
FCM 的 K116/K118 与 F431/M413/M458 具有九寄存器骨架，设备头掩码仍有差异。
其他 M4 九寄存器组也保留位定义差异。A4A8/F4A8 的 FCM1–4 与 FCM_COM 分开登记，
FCM_COM 和 FCM1 同基址；共享窗口使用 fcm_common，不代表另一个频率计。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### cmu：v1

无 PLL 的 RC/晶振时钟与门控；XTAL32/CAN 时钟分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmu_ll_v1.rs` | [cmu_ll_v1](../../data/registers/cmu_ll_v1.yaml) | F120: CMU | 13 / 39 | 本档位对照基准；完整定义见来源 YAML |
| `cmu_ll_v1_1.rs` | [cmu_ll_v1_1](../../data/registers/cmu_ll_v1_1.yaml) | M120: CMU | 13 / 40 | 相对 cmu_ll_v1：定义差异 FCG |
| `cmu_ll_v1_2.rs` | [cmu_ll_v1_2](../../data/registers/cmu_ll_v1_2.yaml) | F115: CMU | 16 / 45 | 相对 cmu_ll_v1：增加 XTAL32CFGR/XTAL32CR/XTAL32NFR；定义差异 FCG/OSCSTBSR |
| `cmu_ll_v1_3.rs` | [cmu_ll_v1_3](../../data/registers/cmu_ll_v1_3.yaml) | F155: CMU | 16 / 47 | 相对 cmu_ll_v1：增加 XTAL32CFGR/XTAL32CR/XTAL32NFR；定义差异 FCG/OSCSTBSR |
| `cmu_ll_v1_4.rs` | [cmu_ll_v1_4](../../data/registers/cmu_ll_v1_4.yaml) | K116/K118: CMU | 17 / 49 | 相对 cmu_ll_v1：增加 CANCKCFGR/XTAL32CFGR/XTAL32CR/XTAL32NFR；定义差异 FCG/OSCSTBSR/XTALCFGR |

### cmu：v2

单 PLL、保留 CMU.FCG 门控。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmu_ll_v2.rs` | [cmu_ll_v2](../../data/registers/cmu_ll_v2.yaml) | F431/M413: CMU | 16 / 58 | 本档位对照基准；完整定义见来源 YAML |
| `cmu_ll_v2_1.rs` | [cmu_ll_v2_1](../../data/registers/cmu_ll_v2_1.yaml) | M458: CMU | 16 / 62 | 相对 cmu_ll_v2：定义差异 FCG/PLLCFGR/SCFGR |

### cmu：v3

PLLH 与扩展输出/晶振分频控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmu_ll_v3.rs` | [cmu_ll_v3](../../data/registers/cmu_ll_v3.yaml) | M441: CMU | 25 / 44 | 本档位对照基准；完整定义见来源 YAML |
| `cmu_ll_v3_1.rs` | [cmu_ll_v3_1](../../data/registers/cmu_ll_v3_1.yaml) | F334/F336: CMU | 26 / 46 | 相对 cmu_ll_v3：增加 CANCKCFGR；定义差异 PLLHCFGR/XTALCFGR/XTALDIVR |
| `cmu_ll_v3_2.rs` | [cmu_ll_v3_2](../../data/registers/cmu_ll_v3_2.yaml) | A448/F448: CMU | 26 / 47 | 相对 cmu_ll_v3：增加 CANCKCFGR；定义差异 SCFGR |
| `cmu_ll_v3_3.rs` | [cmu_ll_v3_3](../../data/registers/cmu_ll_v3_3.yaml) | A472/F472: CMU | 27 / 49 | 相对 cmu_ll_v3：增加 CANCKCFGR/USBCKCFGR；定义差异 PLLHCFGR/SCFGR/XTALCFGR/XTALDIVR |

### cmu：v4

双 PLL，外设/USB/音频时钟；PLLH+PLLA 与 PLL+UPLL 为不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `cmu_ll_v4.rs` | [cmu_ll_v4](../../data/registers/cmu_ll_v4.yaml) | A460/F451/F452/F460: CMU | 27 / 55 | 本档位对照基准；完整定义见来源 YAML |
| `cmu_ll_v4_1.rs` | [cmu_ll_v4_1](../../data/registers/cmu_ll_v4_1.yaml) | A4A0/F467/F4A0/F4A2: CMU | 29 / 57 | 相对 cmu_ll_v4：增加 CANCKCFGR/PLLACFGR/PLLACR/PLLHCFGR/PLLHCR 等 6 项；缺少 PLLCFGR/PLLCR/UPLLCFGR/UPLLCR；定义差异 CKSWR/HRCCR/HRCTRM/I2SCKSEL/LRCCR 等 23 项 |
| `cmu_ll_v4_2.rs` | [cmu_ll_v4_2](../../data/registers/cmu_ll_v4_2.yaml) | A4A8/F4A8: CMU | 31 / 62 | 相对 cmu_ll_v4：增加 CANCKCFGR/PLLACFGR/PLLACR/PLLHCFGR/PLLHCR 等 8 项；缺少 PLLCFGR/PLLCR/UPLLCFGR/UPLLCR；定义差异 CKSWR/HRCCR/HRCTRM/I2SCKSEL/LRCCR 等 23 项 |

### csu：v1

时钟切换控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `csu_ll_v1.rs` | [csu_ll_v1](../../data/registers/csu_ll_v1.yaml) | F175: CSU0, CSU1, CSU2 | 7 / 18 | 本档位对照基准；完整定义见来源 YAML |

### ctc：v1

时钟跟踪控制；可用位与 SDK 宏差异按系列保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ctc_ll_v1.rs` | [ctc_ll_v1](../../data/registers/ctc_ll_v1.yaml) | A4A0/A4A8/F115/F120/F467/F4A0/F4A2/F4A8/M120: CTC | 3 / 11 | 本档位对照基准；完整定义见来源 YAML |
| `ctc_ll_v1_1.rs` | [ctc_ll_v1_1](../../data/registers/ctc_ll_v1_1.yaml) | A448/F155/F334/F336/F448/M441: CTC | 4 / 13 | 相对 ctc_ll_v1：增加 CNT；定义差异 CR1 |
| `ctc_ll_v1_2.rs` | [ctc_ll_v1_2](../../data/registers/ctc_ll_v1_2.yaml) | K116/K118: CTC | 4 / 13 | 相对 ctc_ll_v1：增加 CNT；定义差异 CR1 |
| `ctc_ll_v1_3.rs` | [ctc_ll_v1_3](../../data/registers/ctc_ll_v1_3.yaml) | A472/F472: CTC | 4 / 14 | 相对 ctc_ll_v1：增加 CNT；定义差异 CR1 |

### ctrim：v1

计数/捕获时钟校准；FLIM 位宽、校准源与控制位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `ctrim_ll_v1.rs` | [ctrim_ll_v1](../../data/registers/ctrim_ll_v1.yaml) | F175: CTRIM | 10 / 33 | 本档位对照基准；完整定义见来源 YAML |

### fcm：v1

频率比较/计数；独立状态窗口和位定义分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `fcm_ll_v1.rs` | [fcm_ll_v1](../../data/registers/fcm_ll_v1.yaml) | A4A8/F4A8: FCM1, FCM2, FCM3, FCM4 | 7 / 19 | 本档位对照基准；完整定义见来源 YAML |
| `fcm_ll_v1_1.rs` | [fcm_ll_v1_1](../../data/registers/fcm_ll_v1_1.yaml) | A448/A460/A472/A4A0/F334/F336/F448/F451/F452/F460/F467/F472/F4A0/F4A2/M441: FCM | 9 / 23 | 相对 fcm_ll_v1：增加 SR/STR；定义差异 RIER |
| `fcm_ll_v1_2.rs` | [fcm_ll_v1_2](../../data/registers/fcm_ll_v1_2.yaml) | F431/K116/K118/M413/M458: FCM | 9 / 23 | 相对 fcm_ll_v1：增加 SR/STR |

### fcm_common：v1

频率比较共享启动/状态窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `fcm_common_ll_v1.rs` | [fcm_common_ll_v1](../../data/registers/fcm_common_ll_v1.yaml) | A4A8/F4A8: FCM_COM | 2 / 16 | 本档位对照基准；完整定义见来源 YAML |

### icg：v1

上电配置窗口；配置目标不同，不构成单一复杂度序列。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `icg_ll_v1.rs` | [icg_ll_v1](../../data/registers/icg_ll_v1.yaml) | A448/A4A0/F448/F467/F4A0/F4A2/M441: ICG | 3 / 17 | 本档位对照基准；完整定义见来源 YAML |
| `icg_ll_v1_1.rs` | [icg_ll_v1_1](../../data/registers/icg_ll_v1_1.yaml) | F334/F336: ICG | 3 / 18 | 相对 icg_ll_v1：定义差异 ICG3 |
| `icg_ll_v1_2.rs` | [icg_ll_v1_2](../../data/registers/icg_ll_v1_2.yaml) | M458: ICG | 4 / 17 | 相对 icg_ll_v1：增加 ICG2/ICG4；缺少 ICG3；定义差异 ICG1 |
| `icg_ll_v1_3.rs` | [icg_ll_v1_3](../../data/registers/icg_ll_v1_3.yaml) | A472/F472: ICG | 4 / 17 | 相对 icg_ll_v1：增加 ICG4 |
| `icg_ll_v1_4.rs` | [icg_ll_v1_4](../../data/registers/icg_ll_v1_4.yaml) | F431/M413: ICG | 5 / 21 | 相对 icg_ll_v1：增加 ICG2/ICG4；定义差异 ICG1/ICG3 |
| `icg_ll_v1_5.rs` | [icg_ll_v1_5](../../data/registers/icg_ll_v1_5.yaml) | A4A8/F4A8: ICG | 6 / 23 | 相对 icg_ll_v1：增加 ICG8A/ICG8B/ICG8C |
| `icg_ll_v1_6.rs` | [icg_ll_v1_6](../../data/registers/icg_ll_v1_6.yaml) | F155: ICG | 7 / 15 | 相对 icg_ll_v1：增加 ICG2/ICG4/ICG5/ICG6；定义差异 ICG0/ICG1/ICG3 |
| `icg_ll_v1_7.rs` | [icg_ll_v1_7](../../data/registers/icg_ll_v1_7.yaml) | F120: ICG | 7 / 19 | 相对 icg_ll_v1：增加 ICG2/ICG4/ICG5/ICG6；定义差异 ICG0/ICG1/ICG3 |
| `icg_ll_v1_8.rs` | [icg_ll_v1_8](../../data/registers/icg_ll_v1_8.yaml) | F115: ICG | 7 / 20 | 相对 icg_ll_v1：增加 ICG2/ICG4/ICG5/ICG6；定义差异 ICG0/ICG1/ICG3 |
| `icg_ll_v1_9.rs` | [icg_ll_v1_9](../../data/registers/icg_ll_v1_9.yaml) | M120: ICG | 8 / 19 | 相对 icg_ll_v1：增加 ICG2/ICG4/ICG5/ICG6/ICG7；定义差异 ICG0/ICG1/ICG3 |
| `icg_ll_v1_10.rs` | [icg_ll_v1_10](../../data/registers/icg_ll_v1_10.yaml) | A460/F451/F452/F460: ICG | 8 / 21 | 相对 icg_ll_v1：增加 ICG2/ICG4/ICG5/ICG6/ICG7；定义差异 ICG1/ICG3 |

### icga：v1

上电配置 A 窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `icga_ll_v1.rs` | [icga_ll_v1](../../data/registers/icga_ll_v1.yaml) | K116/K118: ICGA | 15 / 270 | 本档位对照基准；完整定义见来源 YAML |

### icgb：v1

上电配置 B 窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `icgb_ll_v1.rs` | [icgb_ll_v1](../../data/registers/icgb_ll_v1.yaml) | K116/K118: ICGB | 24 / 192 | 本档位对照基准；完整定义见来源 YAML |

### peric：v1

定时器同步/串口及接口使能；目标阵容按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `peric_ll_v1.rs` | [peric_ll_v1](../../data/registers/peric_ll_v1.yaml) | K116/K118: PERIC | 1 / 2 | 本档位对照基准；完整定义见来源 YAML |
| `peric_ll_v1_1.rs` | [peric_ll_v1_1](../../data/registers/peric_ll_v1_1.yaml) | F431/M413: PERIC | 2 / 12 | 相对 peric_ll_v1：增加 USART1_NFC；定义差异 TMR_SYNENR |
| `peric_ll_v1_2.rs` | [peric_ll_v1_2](../../data/registers/peric_ll_v1_2.yaml) | M441: PERIC | 2 / 16 | 相对 peric_ll_v1：增加 USART1_NFC；定义差异 TMR_SYNENR |
| `peric_ll_v1_3.rs` | [peric_ll_v1_3](../../data/registers/peric_ll_v1_3.yaml) | F334/F336: PERIC | 2 / 22 | 相对 peric_ll_v1：增加 USART1_NFC；定义差异 TMR_SYNENR |
| `peric_ll_v1_4.rs` | [peric_ll_v1_4](../../data/registers/peric_ll_v1_4.yaml) | M458: PERIC | 3 / 14 | 相对 peric_ll_v1：增加 SMC_ENAR/USART1_NFC；定义差异 TMR_SYNENR |
| `peric_ll_v1_5.rs` | [peric_ll_v1_5](../../data/registers/peric_ll_v1_5.yaml) | A448/F448: PERIC | 3 / 17 | 相对 peric_ll_v1：增加 SMC_ENAR/USART1_NFC；定义差异 TMR_SYNENR |

### peric：v2

多外设系统控制与存储接口使能。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `peric_ll_v2.rs` | [peric_ll_v2](../../data/registers/peric_ll_v2.yaml) | A460/F451/F452/F460: PERIC | 2 / 4 | 本档位对照基准；完整定义见来源 YAML |
| `peric_ll_v2_1.rs` | [peric_ll_v2_1](../../data/registers/peric_ll_v2_1.yaml) | A472/F472: PERIC | 4 / 14 | 相对 peric_ll_v2：增加 CAN_SYCTLREG/SMC_ENAR/USART1_NFC/USB_SYCTLREG；缺少 SDIOC_SYCTLREG/USBFS_SYCTLREG |
| `peric_ll_v2_2.rs` | [peric_ll_v2_2](../../data/registers/peric_ll_v2_2.yaml) | A4A0/F467/F4A0: PERIC | 8 / 19 | 相对 peric_ll_v2：增加 DMC_ENAR/NFC_ENAR/NFC_STCR/NFC_STSR/SMC_ENAR 等 7 项；缺少 USBFS_SYCTLREG |
| `peric_ll_v2_3.rs` | [peric_ll_v2_3](../../data/registers/peric_ll_v2_3.yaml) | F4A2: PERIC | 9 / 21 | 相对 peric_ll_v2：增加 CAN_SYCTLREG/DMC_ENAR/NFC_ENAR/NFC_STCR/NFC_STSR 等 8 项；缺少 USBFS_SYCTLREG |
| `peric_ll_v2_4.rs` | [peric_ll_v2_4](../../data/registers/peric_ll_v2_4.yaml) | A4A8/F4A8: PERIC | 10 / 65 | 相对 peric_ll_v2：增加 CAN_SYCTLREG/ETH_MAC_IFCONFR/EXMC_ENAR/NFC_STCR/NFC_STSR 等 9 项；缺少 USBFS_SYCTLREG |

### rmu：v1

仅复位状态。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rmu_ll_v1.rs` | [rmu_ll_v1](../../data/registers/rmu_ll_v1.yaml) | F115/F120/F155/M120: RMU | 1 / 10 | 本档位对照基准；完整定义见来源 YAML |
| `rmu_ll_v1_1.rs` | [rmu_ll_v1_1](../../data/registers/rmu_ll_v1_1.yaml) | K116/K118: RMU | 1 / 13 | 相对 rmu_ll_v1：定义差异 RSTF0 |
| `rmu_ll_v1_2.rs` | [rmu_ll_v1_2](../../data/registers/rmu_ll_v1_2.yaml) | A460/F451/F452/F460: RMU | 1 / 16 | 相对 rmu_ll_v1：定义差异 RSTF0 |

### rmu：v2

外设复位控制，分组规模及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rmu_ll_v2.rs` | [rmu_ll_v2](../../data/registers/rmu_ll_v2.yaml) | A4A0/F467/F4A0/F4A2: RMU | 2 / 18 | 本档位对照基准；完整定义见来源 YAML |
| `rmu_ll_v2_1.rs` | [rmu_ll_v2_1](../../data/registers/rmu_ll_v2_1.yaml) | F431/M413: RMU | 2 / 38 | 相对 rmu_ll_v2：增加 FRST0；缺少 PRSTCR0；定义差异 RSTF0 |

### rmu：v3

外设复位控制，分组规模及字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rmu_ll_v3.rs` | [rmu_ll_v3](../../data/registers/rmu_ll_v3.yaml) | M458: RMU | 5 / 41 | 本档位对照基准；完整定义见来源 YAML |
| `rmu_ll_v3_1.rs` | [rmu_ll_v3_1](../../data/registers/rmu_ll_v3_1.yaml) | M441: RMU | 6 / 49 | 相对 rmu_ll_v3：增加 PRSTCR0；定义差异 FRST0/FRST1/FRST2/FRST3/RSTF0 |
| `rmu_ll_v3_2.rs` | [rmu_ll_v3_2](../../data/registers/rmu_ll_v3_2.yaml) | F334/F336: RMU | 6 / 51 | 相对 rmu_ll_v3：增加 PRSTCR0；定义差异 FRST0/FRST1/FRST2/FRST3/RSTF0 |
| `rmu_ll_v3_3.rs` | [rmu_ll_v3_3](../../data/registers/rmu_ll_v3_3.yaml) | A448/F448: RMU | 6 / 54 | 相对 rmu_ll_v3：增加 PRSTCR0；定义差异 FRST0/FRST1/FRST2/FRST3/RSTF0 |
| `rmu_ll_v3_4.rs` | [rmu_ll_v3_4](../../data/registers/rmu_ll_v3_4.yaml) | A472/F472: RMU | 6 / 68 | 相对 rmu_ll_v3：增加 PRSTCR0；定义差异 FRST0/FRST1/FRST2/FRST3/RSTF0 |
| `rmu_ll_v3_5.rs` | [rmu_ll_v3_5](../../data/registers/rmu_ll_v3_5.yaml) | A4A8/F4A8: RMU | 6 / 118 | 相对 rmu_ll_v3：增加 PRSTCR0；定义差异 FRST0/FRST1/FRST2/FRST3/RSTF0 |

### sysctrl：v1

旧式系统时钟与外设门控。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sysctrl_ll_v1.rs` | [sysctrl_ll_v1](../../data/registers/sysctrl_ll_v1.yaml) | F175: SYSCTRL | 25 / 205 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
