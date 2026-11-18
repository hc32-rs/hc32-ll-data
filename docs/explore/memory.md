# Flash、RAM 与 SRAM 控制

EFM 由基础编程/保护、映射或专有读保护扩展、ECC 错误记录到程序/数据 Flash 双窗口分档。
功能维度不同的分支用后缀区分，不把文件发现顺序当成功能排序。
SRAMC 按校验/等待控制、单 ECC 地址窗口、多 ECC 地址窗口分档；bank 与注错位仍逐布局保留。
F175 的 SRAM 控制器使用 sram 名称，其 DFM 纳入本次；M458 的 DFM 保持排除。

M458 的 DFM@0x40050000、EFM2@0x01001C50、DFLASH@0x40058000 是不同地址。
EFM2 的 PN 窗口独立命名为 efm2，不因 M458 的 DFM 被排除而遗漏。
外设版本描述控制器寄存器，容量和地址来自链接脚本，区分全片、bootloader 和 RAM 调试工程。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### efm：v1

基础编程、擦除、状态与保护。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `efm_ll_v1.rs` | [efm_ll_v1](../../data/registers/efm_ll_v1.yaml) | F115/F120: EFM | 14 / 31 | 本档位对照基准；完整定义见来源 YAML |
| `efm_ll_v1_1.rs` | [efm_ll_v1_1](../../data/registers/efm_ll_v1_1.yaml) | M120: EFM | 14 / 33 | 相对 efm_ll_v1：定义差异 FPMTEW/FPMTSW/FRMC |
| `efm_ll_v1_2.rs` | [efm_ll_v1_2](../../data/registers/efm_ll_v1_2.yaml) | F155: EFM | 14 / 34 | 相对 efm_ll_v1：定义差异 FPMTEW/FPMTSW/FRMC/FSCLR/FSR 等 6 项 |

### efm：v2

编程/保护及映射或专有读保护扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `efm_ll_v2.rs` | [efm_ll_v2](../../data/registers/efm_ll_v2.yaml) | A460/F451/F452/F460: EFM | 16 / 36 | 本档位对照基准；完整定义见来源 YAML |
| `efm_ll_v2_1.rs` | [efm_ll_v2_1](../../data/registers/efm_ll_v2_1.yaml) | F431/M413: EFM | 16 / 38 | 相对 efm_ll_v2：增加 FCRCDOR/FOCDPRTFG/FPCROPFG/HRCCFGR；缺少 FSWP/MMF_REMCR0/MMF_REMCR1/MMF_REMPRT；定义差异 FPMTEW/FPMTSW/FRMC/FSCLR/FSR 等 8 项 |
| `efm_ll_v2_2.rs` | [efm_ll_v2_2](../../data/registers/efm_ll_v2_2.yaml) | A448/F334/F336/F448: EFM | 19 / 75 | 相对 efm_ll_v2：增加 CHIPID/F0NWPRT/KEY1/KEY2/WLOCK；缺少 FPMTEW/FPMTSW；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 11 项 |
| `efm_ll_v2_3.rs` | [efm_ll_v2_3](../../data/registers/efm_ll_v2_3.yaml) | M441: EFM | 19 / 76 | 相对 efm_ll_v2：增加 F0NWPRT/KEY1/KEY2/PTNMFG/WLOCK；缺少 FPMTEW/FPMTSW；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 11 项 |
| `efm_ll_v2_4.rs` | [efm_ll_v2_4](../../data/registers/efm_ll_v2_4.yaml) | A472/F472: EFM | 20 / 114 | 相对 efm_ll_v2：增加 CHIPID/F0NWPRT/F1NWPRT/KEY1/KEY2 等 6 项；缺少 FPMTEW/FPMTSW；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 8 项 |
| `efm_ll_v2_5.rs` | [efm_ll_v2_5](../../data/registers/efm_ll_v2_5.yaml) | F467: EFM | 20 / 164 | 相对 efm_ll_v2：增加 F0NWPRT0/F0NWPRT1/F0NWPRT2/F0NWPRT3/KEY1 等 7 项；缺少 FPMTEW/FPMTSW/FSWP；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 7 项 |
| `efm_ll_v2_6.rs` | [efm_ll_v2_6](../../data/registers/efm_ll_v2_6.yaml) | A4A0/F4A0/F4A2: EFM | 26 / 305 | 相对 efm_ll_v2：增加 CHIPID/F0NWPRT0/F0NWPRT1/F0NWPRT2/F0NWPRT3 等 12 项；缺少 FPMTEW/FPMTSW；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 8 项 |

### efm：v3

编程/保护及 ECC 错误记录。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `efm_ll_v3.rs` | [efm_ll_v3](../../data/registers/efm_ll_v3.yaml) | M458: EFM | 22 / 54 | 本档位对照基准；完整定义见来源 YAML |
| `efm_ll_v3_1.rs` | [efm_ll_v3_1](../../data/registers/efm_ll_v3_1.yaml) | A4A8/F4A8: EFM | 50 / 354 | 相对 efm_ll_v3：增加 CHIPID/CHIPSEL/CKCR/CKPR/CKSR 等 40 项；缺少 ERI0/ERI1/ERI2/FCRCDOR/FEC 等 12 项；定义差异 FITE/FRMC/FSCLR/FSR/FSTP 等 8 项 |

### efm：v4

程序/数据 Flash 双控制窗口及 ECC。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `efm_ll_v4.rs` | [efm_ll_v4](../../data/registers/efm_ll_v4.yaml) | K116/K118: EFM | 38 / 224 | 本档位对照基准；完整定义见来源 YAML |

### efm2：v1

独立 Flash PN 窗口。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `efm2_ll_v1.rs` | [efm2_ll_v1](../../data/registers/efm2_ll_v1.yaml) | M458: EFM2 | 4 / 0 | 本档位对照基准；完整定义见来源 YAML |

### sramc：v1

SRAM 校验/等待控制，0 个 ECC 错误地址窗口；bank 与注错位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sramc_ll_v1.rs` | [sramc_ll_v1](../../data/registers/sramc_ll_v1.yaml) | A460/F451/F452/F460: SRAMC | 5 / 20 | 本档位对照基准；完整定义见来源 YAML |
| `sramc_ll_v1_1.rs` | [sramc_ll_v1_1](../../data/registers/sramc_ll_v1_1.yaml) | A4A0/F467/F4A0/F4A2: SRAMC | 5 / 26 | 相对 sramc_ll_v1：定义差异 CKCR/CKSR/WTCR |

### sramc：v2

SRAM 校验/等待控制，1 个 ECC 错误地址窗口；bank 与注错位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sramc_ll_v2.rs` | [sramc_ll_v2](../../data/registers/sramc_ll_v2.yaml) | K116/K118: SRAMC | 7 / 9 | 本档位对照基准；完整定义见来源 YAML |
| `sramc_ll_v2_1.rs` | [sramc_ll_v2_1](../../data/registers/sramc_ll_v2_1.yaml) | M441: SRAMC | 7 / 12 | 相对 sramc_ll_v2：增加 SRAM0_ECCERRADDR/SRAM0_EIBIT0/SRAM0_EIBIT1/SRAM0_EIEN；缺少 SRAM1_ECCERRADDR/SRAM1_EIBIT0/SRAM1_EIBIT1/SRAM1_EIEN；定义差异 CKCR/CKSR |

### sramc：v3

SRAM 校验/等待控制，2 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，3 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，7 个 ECC 错误地址窗口；bank 与注错位分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `sramc_ll_v3.rs` | [sramc_ll_v3](../../data/registers/sramc_ll_v3.yaml) | A448/F448: SRAMC | 11 / 19 | 本档位对照基准；完整定义见来源 YAML |
| `sramc_ll_v3_1.rs` | [sramc_ll_v3_1](../../data/registers/sramc_ll_v3_1.yaml) | A472/F472: SRAMC | 11 / 19 | 相对 sramc_ll_v3：定义差异 SRAM0_ECCERRADDR/SRAMB_ECCERRADDR |
| `sramc_ll_v3_2.rs` | [sramc_ll_v3_2](../../data/registers/sramc_ll_v3_2.yaml) | M458: SRAMC | 13 / 26 | 相对 sramc_ll_v3：增加 SRAM1_ECCERRADDR/SRAM1_EIBIT0/SRAM1_EIBIT1/SRAM1_EIEN/SRAMH_ECCERRADDR 等 10 项；缺少 SRAM0_ECCERRADDR/SRAM0_EIBIT0/SRAM0_EIBIT1/SRAM0_EIEN/SRAMB_ECCERRADDR 等 8 项；定义差异 CKCR/CKSR |
| `sramc_ll_v3_3.rs` | [sramc_ll_v3_3](../../data/registers/sramc_ll_v3_3.yaml) | F334/F336: SRAMC | 15 / 25 | 相对 sramc_ll_v3：增加 SRAMH_ECCERRADDR/SRAMH_EIBIT0/SRAMH_EIBIT1/SRAMH_EIEN；定义差异 CKCR/CKSR/SRAMB_ECCERRADDR/SRAMB_EIBIT0/SRAMB_EIBIT1 等 6 项 |
| `sramc_ll_v3_4.rs` | [sramc_ll_v3_4](../../data/registers/sramc_ll_v3_4.yaml) | A4A8/F4A8: SRAMC | 36 / 61 | 相对 sramc_ll_v3：增加 CACHE_ECCERRADDR/CACHE_EIBIT0/CACHE_EIBIT1/CACHE_EIBIT2/CACHE_EIBIT3 等 29 项；缺少 SRAM0_ECCERRADDR/SRAM0_EIBIT0/SRAM0_EIBIT1/SRAM0_EIEN；定义差异 CKCR/CKSR/SRAMB_ECCERRADDR/SRAMB_EIBIT0/SRAMB_EIBIT1 等 6 项 |
<!-- peripheral-versions:end -->

## GCC 链接脚本容量

下表列出 27 套 SDK 中设备目录下常规按型号命名的 GCC 脚本。
Flash 起始地址均为 0x00000000，文件名加 `.ld` 即可在相应 SDK 中定位。

| GCC 脚本 | Flash | RAM | 备份 RAM / 数据 Flash |
|---|---|---|---|
| HC32A448xA | 128K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32A448xC | 256K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32A460xC | 256K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32A460xE | 512K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32A472xE | 256K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32A4A0xG | 1M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32A4A0xI | 2M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32A4A8xI | 2M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F115x8 | 64K | 4K @ 0x20000000 | — |
| HC32F120x6 | 32K | 4K @ 0x20000000 | — |
| HC32F120x8 | 64K | 4K @ 0x20000000 | — |
| HC32F155xA | 128K | 32K @ 0x20000000 | — |
| HC32F155xC | 256K | 32K @ 0x20000000 | — |
| HC32F175xE | 512K | 48K @ 0x20000000 | — |
| HC32F334x8 | 64K | 32K @ 0x1FFFC000 | RAMB 4K @ 0x200F0000 |
| HC32F334xA | 128K | 32K @ 0x1FFFC000 | RAMB 4K @ 0x200F0000 |
| HC32F336xA | 128K | 32K @ 0x1FFFC000 | RAMB 4K @ 0x200F0000 |
| HC32F431xA | 128K | 16K @ 0x1FFFE000 | — |
| HC32F448xA | 128K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32F448xC | 256K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32F451xE | 512K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32F452xE | 512K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32F460xC | 256K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32F460xE | 512K | 188K @ 0x1FFF8000 | RET_RAM 4K @ 0x200F0000 |
| HC32F467xG | 1M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F472xE | 256K | 64K @ 0x1FFF8000 | RAMB 4K @ 0x200F0000 |
| HC32F4A0xG | 1M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F4A0xI | 2M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F4A2xG | 1M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F4A2xI | 2M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32F4A8xI | 2M | 512K @ 0x1FFE0000 | RAMB 4K @ 0x200F0000 |
| HC32K116xA | 128K | 32K @ 0x20000000 | DFLASH 32K @ 0x40018000 |
| HC32K118xC | 256K | 32K @ 0x20000000 | DFLASH 32K @ 0x40018000 |
| HC32M120x6 | 32K | 4K @ 0x20000000 | — |
| HC32M413xA | 128K | 16K @ 0x1FFFE000 | — |
| HC32M441xA | 128K | 32K @ 0x1FFFC000 | — |
| HC32M441xC | 256K | 32K @ 0x1FFFC000 | — |
| HC32M458xC | 256K | 64K @ 0x1FFF8000 | DFLASH 8K @ 0x40058000 |
| HC32M458xE | 512K | 64K @ 0x1FFF8000 | DFLASH 8K @ 0x40058000 |

型号完整容量、双 Bank 与工程专用配置以相应手册和链接脚本为准。
