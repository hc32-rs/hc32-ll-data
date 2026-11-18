# CRC、AES、TRNG、RTC 与 CAN

CRC 按旧式单数据输入、数据窗口阵列、双结果分档；独立 FLG 与位域方言保留后缀。
AES 按 AES-128 与含 KEYSIZE 的扩展分档。TRNG 的 M458 ENDIE 差异独立保留。
RTC 从带误差补偿的基础日历/闹钟到时间捕获；TPCR 与 TSTR/WUTR 是不同分支，
实际寄存器与位域仍以来源为准。

CAN 的基础布局之后是含 FBT/TDC 的数据相位控制，再到 RAM 校验/注错。
CAN 与 MCAN 为独立外设，A4A8/F4A8 同时包含两种；MCANRAM 为报文 RAM ECC 窗口。
MCAN 的 UTSU 增量以布局后缀保留，核验依据为 hc32_ll_can.h / hc32_ll_mcan.h。
PKE 不在 README 的排除清单中，本次以独立公钥运算寄存器块登记基础版本。

HASH、SKE、USB 等 README M4 扩展项保持排除，详见[范围与对照](../layout-versions.md)。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### aes：v1

AES 基础密钥控制；CRDM 镜像按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aes_ll_v1.rs` | [aes_ll_v1](../../data/registers/aes_ll_v1.yaml) | A460/F451/F452/F460: AES | 9 / 2 | 本档位对照基准；完整定义见来源 YAML |

### aes：v2

AES 密钥长度控制扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `aes_ll_v2.rs` | [aes_ll_v2](../../data/registers/aes_ll_v2.yaml) | A448/A472/A4A0/F448/F467/F472/F4A0/F4A2: AES | 13 / 3 | 本档位对照基准；完整定义见来源 YAML |

### can：v1

基础 CAN → FBT/TDC 数据相位控制 → RAM 校验/注错。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `can_ll_v1.rs` | [can_ll_v1](../../data/registers/can_ll_v1.yaml) | A460/F452/F460: CAN | 23 / 85 | 本档位对照基准；完整定义见来源 YAML |

### can：v2

基础 CAN → FBT/TDC 数据相位控制 → RAM 校验/注错。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `can_ll_v2.rs` | [can_ll_v2](../../data/registers/can_ll_v2.yaml) | A472/F472: CAN1, CAN2, CAN3；A4A0/F467/F4A0/F4A2: CAN1, CAN2 | 25 / 100 | 本档位对照基准；完整定义见来源 YAML |

### can：v3

基础 CAN → FBT/TDC 数据相位控制 → RAM 校验/注错。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `can_ll_v3.rs` | [can_ll_v3](../../data/registers/can_ll_v3.yaml) | A4A8/F4A8: CAN1, CAN2 | 31 / 106 | 本档位对照基准；完整定义见来源 YAML |

### crc：v1

数据输入窗口阵列；双结果/FLG 按能力分档与变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `crc_ll_v1.rs` | [crc_ll_v1](../../data/registers/crc_ll_v1.yaml) | A448/F334/F336/F448/K116/K118/M441: CRC | 34 / 2 | 本档位对照基准；完整定义见来源 YAML |
| `crc_ll_v1_1.rs` | [crc_ll_v1_1](../../data/registers/crc_ll_v1_1.yaml) | A472/A4A0/F115/F120/F155/F467/F472/F4A0/F4A2/M120: CRC | 34 / 2 | 相对 crc_ll_v1：定义差异 DAT0/DAT1/DAT10/DAT11/DAT12 等 32 项 |
| `crc_ll_v1_2.rs` | [crc_ll_v1_2](../../data/registers/crc_ll_v1_2.yaml) | A460/F451/F452/F460: CRC | 35 / 7 | 相对 crc_ll_v1：增加 FLG；定义差异 CR/DAT0/DAT1/DAT10/DAT11 等 34 项 |

### crc：v2

数据输入窗口阵列；双结果/FLG 按能力分档与变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `crc_ll_v2.rs` | [crc_ll_v2](../../data/registers/crc_ll_v2.yaml) | A4A8/F431/F4A8/M413/M458: CRC | 35 / 3 | 本档位对照基准；完整定义见来源 YAML |

### mcan：v1

MCAN 控制；UTSU 等字段分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `mcan_ll_v1.rs` | [mcan_ll_v1](../../data/registers/mcan_ll_v1.yaml) | K116/K118: MCAN | 45 / 493 | 本档位对照基准；完整定义见来源 YAML |
| `mcan_ll_v1_1.rs` | [mcan_ll_v1_1](../../data/registers/mcan_ll_v1_1.yaml) | A448/A4A8/F334/F336/F448/F4A8: MCAN1, MCAN2 | 45 / 494 | 相对 mcan_ll_v1：定义差异 CCCR |

### mcanram：v1

MCAN 报文 RAM 校验/注错。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `mcanram_ll_v1.rs` | [mcanram_ll_v1](../../data/registers/mcanram_ll_v1.yaml) | A4A8/F4A8: MCANRAM | 6 / 6 | 本档位对照基准；完整定义见来源 YAML |
| `mcanram_ll_v1_1.rs` | [mcanram_ll_v1_1](../../data/registers/mcanram_ll_v1_1.yaml) | F334/F336: MCANRAM | 6 / 6 | 相对 mcanram_ll_v1：定义差异 ECCERRADDR |
| `mcanram_ll_v1_2.rs` | [mcanram_ll_v1_2](../../data/registers/mcanram_ll_v1_2.yaml) | K116/K118: MCANRAM | 6 / 6 | 相对 mcanram_ll_v1：定义差异 ECCERRADDR |

### pke：v1

公钥运算引擎独立寄存器块。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `pke_ll_v1.rs` | [pke_ll_v1](../../data/registers/pke_ll_v1.yaml) | A4A8/F4A8: PKE | 23 / 29 | 本档位对照基准；完整定义见来源 YAML |

### rtc：v1

RTC 日历/闹钟与误差补偿，FCLR 分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rtc_ll_v1.rs` | [rtc_ll_v1](../../data/registers/rtc_ll_v1.yaml) | A448/A472/F334/F336/F448/F472/M441: RTC | 16 / 34 | 本档位对照基准；完整定义见来源 YAML |
| `rtc_ll_v1_1.rs` | [rtc_ll_v1_1](../../data/registers/rtc_ll_v1_1.yaml) | F115/F155: RTC | 16 / 34 | 相对 rtc_ll_v1：定义差异 CR1/ERRCRL |
| `rtc_ll_v1_2.rs` | [rtc_ll_v1_2](../../data/registers/rtc_ll_v1_2.yaml) | A460/F451/F452/F460: RTC | 16 / 35 | 相对 rtc_ll_v1：定义差异 CR1/CR2/ERRCRL |
| `rtc_ll_v1_3.rs` | [rtc_ll_v1_3](../../data/registers/rtc_ll_v1_3.yaml) | K116/K118: RTC | 17 / 36 | 相对 rtc_ll_v1：增加 FCLR；定义差异 CR3/ERRCRH |

### rtc：v2

日历/闹钟与时间捕获；TPCR 与 TSTR/WUTR 为同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `rtc_ll_v2.rs` | [rtc_ll_v2](../../data/registers/rtc_ll_v2.yaml) | A4A8/F4A8: RTC | 24 / 58 | 本档位对照基准；完整定义见来源 YAML |
| `rtc_ll_v2_1.rs` | [rtc_ll_v2_1](../../data/registers/rtc_ll_v2_1.yaml) | A4A0/F467/F4A0/F4A2: RTC | 24 / 59 | 相对 rtc_ll_v2：定义差异 CR3/DAYTP/ERRCRL/HOURTP/MINTP 等 7 项 |

### trng：v1

随机数控制；ENDIE 等字段按变体保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `trng_ll_v1.rs` | [trng_ll_v1](../../data/registers/trng_ll_v1.yaml) | A448/A460/A472/A4A0/A4A8/F448/F451/F452/F460/F467/F472/F4A0/F4A2/F4A8: TRNG | 4 / 4 | 本档位对照基准；完整定义见来源 YAML |
| `trng_ll_v1_1.rs` | [trng_ll_v1_1](../../data/registers/trng_ll_v1_1.yaml) | M458: TRNG | 4 / 5 | 相对 trng_ll_v1：定义差异 CR |
<!-- peripheral-versions:end -->
