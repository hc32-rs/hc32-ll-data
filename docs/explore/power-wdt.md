# 电源、低功耗与看门狗

PWC 从基础低功耗/RAM 保持、双路或扩展电压监测、集成门控/掉电唤醒到备份域分档。
F431/M413/M458 的精简 PWC 不再排在全功能型号之后。
FCG 位图、PVD 阈值、RAM 保持和 PWRC 分支按实例保留，不以寄存器总数直接代表功能大小。

F431/M413 的 PWC 在 0x40054000、CMU 在 0x40054400，M458 分配对调。
F467 等备份域含 BKR0–127、VBATCR/VBATRSTR；FCG 位图随外设阵容变化。
M4 低压检测由 PWC.PVD 与 hc32_ll_pwc.h 的 PWC_LVD_* API 表达。

SWDT 基础布局为 SR/RR，通过 ICG 配置；带 CR@0、SR@4、RR@8 的扩展使用后缀。
A460/F451/F452/F460 的 SWDT 为基础布局，其余有 CR。
M4 的 WDT/SWDT 分别在 0x40049000/0x40049400；WDT 保持独立外设名。
F175 的 LVD 另有 LOCK，IWDT/WWDT 不沿用 DDL 编号。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### iwdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `iwdt_ll_v1.rs` | [iwdt_ll_v1](../../data/registers/iwdt_ll_v1.yaml) | F175: IWDT | 5 / 12 | 本档位对照基准；完整定义见来源 YAML |

### lvd：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lvd_ll_v1.rs` | [lvd_ll_v1](../../data/registers/lvd_ll_v1.yaml) | F175: LVD | 3 / 13 | 本档位对照基准；完整定义见来源 YAML |

### pwc：v1

基础低功耗、RAM 保持与 LVD。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pwc_ll_v1.rs` | [pwc_ll_v1](../../data/registers/pwc_ll_v1.yaml) | F115/F120: PWC | 6 / 23 | 本档位对照基准；完整定义见来源 YAML |
| `pwc_ll_v1_1.rs` | [pwc_ll_v1_1](../../data/registers/pwc_ll_v1_1.yaml) | M120: PWC | 6 / 24 | 相对 pwc_ll_v1：定义差异 PWRC |
| `pwc_ll_v1_2.rs` | [pwc_ll_v1_2](../../data/registers/pwc_ll_v1_2.yaml) | F155: PWC | 7 / 27 | 相对 pwc_ll_v1：增加 PWRMON；定义差异 DBGC/PWRC/STPMCR |

### pwc：v2

双路/扩展电压检测与低功耗控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pwc_ll_v2.rs` | [pwc_ll_v2](../../data/registers/pwc_ll_v2.yaml) | K116/K118: PWC | 10 / 34 | 本档位对照基准；完整定义见来源 YAML |
| `pwc_ll_v2_1.rs` | [pwc_ll_v2_1](../../data/registers/pwc_ll_v2_1.yaml) | M458: PWC | 11 / 39 | 相对 pwc_ll_v2：增加 PVDCR0/PVDCR1/PVDDSR/PVDFCR/PVDICR 等 6 项；缺少 LVD2CR/LVDCSR/PWRC/PWRMON/RAMCR；定义差异 DBGC/FPRC/LPMCSCR/LPMCSR/STPMCR |
| `pwc_ll_v2_2.rs` | [pwc_ll_v2_2](../../data/registers/pwc_ll_v2_2.yaml) | F431/M413: PWC | 12 / 43 | 相对 pwc_ll_v2：增加 PVDCR0/PVDCR1/PVDDSR/PVDFCR/PVDICR 等 6 项；缺少 LVD2CR/LVDCSR/PWRC/PWRMON；定义差异 DBGC/FPRC/LPMCSCR/LPMCSR/RAMCR 等 6 项 |

### pwc：v3

集成外设门控、掉电唤醒与电源模式。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pwc_ll_v3.rs` | [pwc_ll_v3](../../data/registers/pwc_ll_v3.yaml) | A472/F472: PWC | 27 / 175 | 本档位对照基准；完整定义见来源 YAML |
| `pwc_ll_v3_1.rs` | [pwc_ll_v3_1](../../data/registers/pwc_ll_v3_1.yaml) | F334/F336: PWC | 29 / 133 | 相对 pwc_ll_v3：增加 LPMCSCR/LPMCSR；定义差异 FCG0/FCG1/FCG2/FCG3/FPRC 等 9 项 |
| `pwc_ll_v3_2.rs` | [pwc_ll_v3_2](../../data/registers/pwc_ll_v3_2.yaml) | F451: PWC | 29 / 151 | 相对 pwc_ll_v3：增加 MDSWCR/PWCMR/PWRC2/PWRC3/RAMOPM 等 6 项；缺少 PRAMLPC/PWRC4/PWRC5/PWRC6；定义差异 FCG0/FCG1/FCG2/FCG3/PDWKE0 等 20 项 |
| `pwc_ll_v3_3.rs` | [pwc_ll_v3_3](../../data/registers/pwc_ll_v3_3.yaml) | A460/F452/F460: PWC | 29 / 152 | 相对 pwc_ll_v3：增加 MDSWCR/PWCMR/PWRC2/PWRC3/RAMOPM 等 6 项；缺少 PRAMLPC/PWRC4/PWRC5/PWRC6；定义差异 FCG0/FCG1/FCG2/FCG3/PDWKE0 等 20 项 |
| `pwc_ll_v3_4.rs` | [pwc_ll_v3_4](../../data/registers/pwc_ll_v3_4.yaml) | M441: PWC | 30 / 131 | 相对 pwc_ll_v3：增加 PWRC2/PWRC3/RAMOPM；定义差异 FCG0/FCG1/FCG2/FCG3/FPRC 等 12 项 |
| `pwc_ll_v3_5.rs` | [pwc_ll_v3_5](../../data/registers/pwc_ll_v3_5.yaml) | A448/F448: PWC | 30 / 142 | 相对 pwc_ll_v3：增加 PWRC2/PWRC3/RAMOPM；定义差异 FCG0/FCG1/FCG2/FCG3/FPRC 等 11 项 |

### pwc：v4

备份域 BKR/VBAT、唤醒源、电压监测及门控。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pwc_ll_v4.rs` | [pwc_ll_v4](../../data/registers/pwc_ll_v4.yaml) | F467: PWC | 160 / 214 | 本档位对照基准；完整定义见来源 YAML |
| `pwc_ll_v4_1.rs` | [pwc_ll_v4_1](../../data/registers/pwc_ll_v4_1.yaml) | A4A0/F4A0/F4A2: PWC | 160 / 224 | 相对 pwc_ll_v4：定义差异 FCG1/FCG2/FCG3 |
| `pwc_ll_v4_2.rs` | [pwc_ll_v4_2](../../data/registers/pwc_ll_v4_2.yaml) | A4A8/F4A8: PWC | 164 / 235 | 相对 pwc_ll_v4：增加 LPMCSCR/LPMCSR/VBATPORF/VBATPORFCLR；定义差异 FCG0/FCG1/FCG2/FCG3/PRAMLPC 等 6 项 |

### swdt：v1

SR/RR 基础看门狗；带 CR 的扩展布局使用后缀。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `swdt_ll_v1.rs` | [swdt_ll_v1](../../data/registers/swdt_ll_v1.yaml) | A460/F451/F452/F460: SWDT | 2 / 4 | 本档位对照基准；完整定义见来源 YAML |
| `swdt_ll_v1_1.rs` | [swdt_ll_v1_1](../../data/registers/swdt_ll_v1_1.yaml) | A448/A472/A4A0/A4A8/F115/F120/F155/F334/F336/F431/F448/F467/F472/F4A0/F4A2/F4A8/K116/K118/M120/M413/M441/M458: SWDT | 3 / 9 | 相对 swdt_ll_v1：增加 CR |

### wdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `wdt_ll_v1.rs` | [wdt_ll_v1](../../data/registers/wdt_ll_v1.yaml) | A448/A460/A472/A4A0/A4A8/F334/F336/F431/F448/F451/F452/F460/F467/F472/F4A0/F4A2/F4A8/M413/M441/M458: WDT | 3 / 9 | 本档位对照基准；完整定义见来源 YAML |

### wwdt：v1

同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `wwdt_ll_v1.rs` | [wwdt_ll_v1](../../data/registers/wwdt_ll_v1.yaml) | F175: WWDT | 3 / 6 | 本档位对照基准；完整定义见来源 YAML |
<!-- peripheral-versions:end -->
