# 外设版本与布局命名（LL）

本页及各专题中的名称为**待确认方案**。生成器与 `data/registers/` 尚未迁移，
当前实际输入仍由 `data/csv/register_versions.csv` 引用。确认本阶段文档与 CSV 后，才修改生成链。

## 统一规则

- DDL、LL 各自按外设从 v1 开始。主档位依控制、通道、序列和读出能力由简单到复杂排列。
- 同档位的扩展、裁剪、位域或命名分支使用 `_1`、`_2` 数字后缀；不使用字母或描述性版本后缀。
- 目标文件为 `<kind>_ll_vN[_M].rs`。主档位与后缀都不是发布顺序或兼容性承诺。
- 复杂度存在多个维度。同档不同分支在专题说明，后缀只保证布局唯一，不表示严格的功能大小关系。
- 一个现有生成布局保留一个独立目标；本阶段不做寄存器归一化或跨库合并。
  同一旧 kind 下混入的 I2C/I2S、TMR0/2/4/6 按完整实例名恢复外设名。
  模式视图、共享窗口与 derivedFrom 实例保留明确对应，不能用行数推断物理外设数量。
- 旧文档的概念分组不能直接替代生成 IR。仅比较寄存器数量或忽略位域差异的合并建议已删除。
  `common` 也获得明确的基础版本，不再作为拟议版本名。

## 数据与维护

[显式映射](peripheral-version-map.json)是本阶段唯一的编号来源。
[实例对照 CSV](peripheral-versions.csv)及本页/专题的标记区由
`hc32-data-tools/scripts/export_peripheral_versions.py` 生成。脚本不会自行分配或重排编号。
从工作区运行：

```sh
python3 hc32-data-tools/scripts/export_peripheral_versions.py
python3 hc32-data-tools/scripts/export_peripheral_versions.py --check
```

CSV 列固定为 `series,instance,kind,current_version,proposed_version,target_file,status,notes`。
`kind/current_version` 是源数据的真实键，**不是双仓库合并后 PAC 的临时重映射版本**。
完整目标外设名由 `target_file` 和 notes 中的 `target_kind` 给出。
`proposed` 表示待确认，`excluded` 表示本次排除；排除项的新版本与目标文件名为空。
CSV 为 UTF-8、LF，每行对应系列及 SVD 实例，按旧 kind、新版本、目标、系列、实例稳定排序。

映射记录来源 YAML 的 SHA-256 和确切系列/实例集合；来源变化、缺项、重复项、目标冲突或排除范围变化都会报错。
专题列出的寄存器/字段数是当前 IR 的条目数，仅用于核对，不是复杂度评分，也不包括原始 SVD 中未保留的属性。
变体差异提示与同主档位的基础布局比较寄存器名、偏移、访问属性及内联位域，最多列出五个名称。
枚举内容、节点名称和原始复位语义需打开来源核对；提示为空差异不等于可合并。
复位值、设备头支持位、封装与硬件行为仍需读原始来源；命名表不会扩充或修复当前生成数据。

修改分配时先核对 SVD、生成 IR 与专题列出的 SDK 证据，编辑显式映射，再重导出。
确认后不应因加入新系列自动改变既有编号。SDK 只读，SVD 与审计 CSV 本阶段均不修改。

## 本次排除

README 的 M4 扩展项：DCU、MAU、FMAC、HASH、SKE、USB、ETH、MDIO、NFC、SDIOC、DVP、
QSPI、SMC、DMC、MPU、KEYSCAN、OTS、VREF、HRPWM、HALL、PLA、TMR6、ERMU，以及 **M458 的 DFM**。
USB 包括 USBFS/USBHS，TMR6/HRPWM 包括共享窗口。F175 的 DFM、M458 的 EFM2 不在此清单内。
PERIC/PWC/AOS 等仍完整登记，即使包含排除外设的控制位；不裁剪寄存器。
这些排除项在 CSV 和下表保留旧来源，第二阶段维持原命名，不擅自分档。

<!-- peripheral-versions:begin -->
## 覆盖统计

共 1916 个系列/实例映射：1507 个待确认、409 个排除。
纳入 59 个目标外设名、378 个独立生成布局。

| 外设 | 主档位 | 布局数 | 功能依据 |
|---|---|---:|---|
| [adc](explore/analog.md) | v1–v5 | 26 | 旧式 SQR/JQR 四 rank 序列、无内嵌 BGR；SeqA/SeqB、统一采样时间；通道数及 OPACR 分变体；逐通道采样与多结果，平均/同步/看门狗位域按实例保留；逐通道采样，扩展增益/采样保持或运放控制；同档不同分支；逐通道采样、SeqC/差分结果 DIR；OPACR 按实例保留 |
| [aes](explore/crypto-comm.md) | v1–v2 | 2 | AES 基础密钥控制；CRDM 镜像按变体保留；AES 密钥长度控制扩展 |
| [aos](explore/pca-bt-pcnt-aos.md) | v1–v10 | 13 | 基础定时器/ADC 路由，DMA 扩展；增加事件端口与定时器目标；六个 ADC 目标与 DMA/公共触发；增加 DCU/TMRA/TMR4 等路由目标；多定时器、DMA 与事件端口路由；M4 路由矩阵基础；扩展定时器/模拟路由目标；两组 74 寄存器路由，目标阵容不同；87 寄存器路由矩阵；94 寄存器路由矩阵 |
| [atim](explore/timer-adv.md) | v1 | 1 | 完整单块高级定时器，多模式及四通道控制 |
| [can](explore/crypto-comm.md) | v1–v3 | 3 | 基础 CAN → FBT/TDC 数据相位控制 → RAM 校验/注错 |
| [cmp](explore/analog.md) | v1–v4 | 17 | 精简比较器，迟滞/滤波及电压选择；窗口/消隐比较器；TWR/TWSR/BWSR 属不同分支；比较器窗口与同步控制；比较器、DAC 与消隐窗口；同步控制按变体保留 |
| [cmp_common](explore/analog.md) | v1 | 2 | 比较器共享 DAC 数据/控制窗口 |
| [cmu](explore/clock-reset.md) | v1–v4 | 14 | 无 PLL 的 RC/晶振时钟与门控；XTAL32/CAN 时钟分变体；单 PLL、保留 CMU.FCG 门控；PLLH 与扩展输出/晶振分频控制；双 PLL，外设/USB/音频时钟；PLLH+PLLA 与 PLL+UPLL 为不同分支 |
| [crc](explore/crypto-comm.md) | v1–v3 | 5 | 旧式单数据窗口；数据输入窗口阵列；双结果/FLG 按能力分档与变体 |
| [csu](explore/clock-reset.md) | v1 | 1 | 时钟切换控制 |
| [ctc](explore/clock-reset.md) | v1 | 4 | 时钟跟踪控制；可用位与 SDK 宏差异按系列保留 |
| [ctrim](explore/clock-reset.md) | v1 | 1 | 计数/捕获时钟校准；FLIM 位宽、校准源与控制位分变体 |
| [dac](explore/analog.md) | v1–v2 | 4 | 双数据寄存器 DAC；对齐/增益字段分变体；DAC-ADC 硬件触发扩展 |
| [dbgc](explore/gpio-int.md) | v1–v4 | 13 | 停止模式调试控制；MCU 调试状态和停止控制；运行/停止调试与跟踪控制；Flash CRC 调试控制；是否含 MCU 状态按变体保留 |
| [dbgc_t](explore/gpio-int.md) | v1 | 2 | 调试认证/目标控制，ID 与保护字段分变体 |
| [dfm](explore/memory.md) | v1 | 1 | 数据 Flash 控制（仅 F175 纳入） |
| [dma](explore/dma.md) | v1–v2 | 12 | 基本传输；BASE1_LLP/BASE2_LLP 链表扩展分变体；逐通道 LLP 与重复/非连续传输；CHENCLR/HPROT、通道数分变体 |
| [dmac](explore/dma.md) | v1 | 1 | 旧式 CONF/CONFA DMA；4 个源地址窗口，触发位与字段方言分变体 |
| [efm](explore/memory.md) | v1–v4 | 14 | 基础编程、擦除、状态与保护；编程/保护及映射或专有读保护扩展；编程/保护及 ECC 错误记录；程序/数据 Flash 双控制窗口及 ECC |
| [efm2](explore/memory.md) | v1 | 1 | 独立 Flash PN 窗口 |
| [emb](explore/timer-adv.md) | v1–v3 | 17 | 基础刹车 → 释放选择 → 边沿选择；各实例使能源分变体 |
| [emb_ctl1](explore/timer-adv.md) | v1 | 1 | SVD 的零基址附属节点，保留审计映射，不作为可用硬件实例背书 |
| [exti](explore/gpio-int.md) | v1 | 1 | 外部中断控制 |
| [fcm](explore/clock-reset.md) | v1 | 3 | 频率比较/计数；独立状态窗口和位定义分变体 |
| [fcm_common](explore/clock-reset.md) | v1 | 1 | 频率比较共享启动/状态窗口 |
| [gpio](explore/gpio-int.md) | v1–v2 | 15 | 旧式 AFR 端口控制；端口保护与复用；字节端口和 PCR/PFSR 为同档不同分支 |
| [gtim](explore/timer-gen.md) | v1 | 1 | 通用捕获/比较定时器；IER 与控制字段分变体 |
| [i2c](explore/spi-i2c-i2s.md) | v1–v4 | 12 | 旧式地址匹配 → CR 配置/滤波 → 超时 SLTR → FSTR/从地址扩展 |
| [i2s](explore/spi-i2c-i2s.md) | v1 | 4 | 音频串行控制；位域分变体 |
| [icg](explore/clock-reset.md) | v1 | 11 | 上电配置窗口；配置目标不同，不构成单一复杂度序列 |
| [icga](explore/clock-reset.md) | v1 | 1 | 上电配置 A 窗口 |
| [icgb](explore/clock-reset.md) | v1 | 1 | 上电配置 B 窗口 |
| [intc](explore/gpio-int.md) | v1–v3 | 13 | 固定具名向量及外部中断控制；部分通用路由槽与具名向量混合；144 个通用路由槽及外部中断控制 |
| [iwdt](explore/power-wdt.md) | v1 | 1 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [lptim](explore/timer-gen.md) | v1 | 1 | 低功耗计数；CMP 比较为扩展，偏移及控制字段分变体 |
| [lpuart](explore/serial.md) | v1 | 1 | 低功耗串口，SCNT/BSEL/MODU 与 SDECON 控制 |
| [lvd](explore/power-wdt.md) | v1 | 1 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [mcan](explore/crypto-comm.md) | v1 | 2 | MCAN 控制；UTSU 等字段分变体 |
| [mcanram](explore/crypto-comm.md) | v1 | 3 | MCAN 报文 RAM 校验/注错 |
| [peric](explore/clock-reset.md) | v1–v2 | 11 | 定时器同步/串口及接口使能；目标阵容按变体保留；多外设系统控制与存储接口使能 |
| [pke](explore/crypto-comm.md) | v1 | 1 | 公钥运算引擎独立寄存器块 |
| [pwc](explore/power-wdt.md) | v1–v4 | 15 | 基础低功耗、RAM 保持与 LVD；双路/扩展电压检测与低功耗控制；集成外设门控、掉电唤醒与电源模式；备份域 BKR/VBAT、唤醒源、电压监测及门控 |
| [rmu](explore/clock-reset.md) | v1–v3 | 11 | 仅复位状态；外设复位控制，分组规模及字段按变体保留 |
| [rtc](explore/crypto-comm.md) | v1–v2 | 7 | RTC 日历/闹钟与误差补偿，FCLR 分变体；日历/闹钟与时间捕获；TPCR 与 TSTR/WUTR 为同档不同分支 |
| [spi](explore/spi-i2c-i2s.md) | v1–v2 | 8 | 旧式 CR0/CR1、HDOE/SSI；CFG1/CFG2 配置架构；CR/CR1、分频和接收控制分变体 |
| [sram](explore/memory.md) | v1 | 1 | SRAM 错误控制 |
| [sramc](explore/memory.md) | v1–v3 | 9 | SRAM 校验/等待控制，0 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，1 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，2 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，3 个 ECC 错误地址窗口；bank 与注错位分变体；SRAM 校验/等待控制，7 个 ECC 错误地址窗口；bank 与注错位分变体 |
| [swdt](explore/power-wdt.md) | v1 | 2 | SR/RR 基础看门狗；带 CR 的扩展布局使用后缀 |
| [sysctrl](explore/clock-reset.md) | v1 | 1 | 旧式系统时钟与外设门控 |
| [tmr0](explore/timer-gen.md) | v1–v2 | 9 | 单 A 计数通道；双 A/B 计数通道 |
| [tmr2](explore/timer-gen.md) | v1–v2 | 5 | 单通道计数与硬件触发；双通道计数与硬件触发，捕获/同步位分变体 |
| [tmr4](explore/timer-gen.md) | v1–v3 | 12 | 三相基础输出/采样 → 四相扩展 → 多组 OCCR 缓冲；不同相数分支不宣称包含关系 |
| [tmr4_ecer](explore/timer-gen.md) | v1 | 1 | TMR4 共享外部时钟控制 |
| [tmra](explore/timer-gen.md) | v1–v4 | 17 | 2 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留；4 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留；6 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留；8 路比较输出；计数位宽、EPSCR/相位控制及字段按变体保留 |
| [tmrb](explore/timer-gen.md) | v1 | 2 | 基本计数与比较，缺失位域按来源保留 |
| [trng](explore/crypto-comm.md) | v1 | 2 | 随机数控制；ENDIE 等字段按变体保留 |
| [usart](explore/serial.md) | v1–v3 | 35 | 基础串口与流控；LINEN、半双工等按实例保留；复合功能串口：SmartCard、LIN/LBMC 或接收超时，同档不同分支；旧式 CR1 起始布局，TOR/ADDR/IR；与现代复合功能串口同档不同分支；串口扩展：RS485 或自动波特率；SmartCard/LIN 分支分别保留 |
| [wdt](explore/power-wdt.md) | v1 | 1 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |
| [wwdt](explore/power-wdt.md) | v1 | 1 | 同档控制；窗口偏移、锁定、阈值/滤波或字段方言分变体 |

## 排除项对照

| 原生成布局 | 系列：实例 |
|---|---|
| [dcu_v1](../data/registers/dcu_v1.yaml) | A448/F448/M441: DCU1, DCU2, DCU3, DCU4 |
| [dcu_v10](../data/registers/dcu_v10.yaml) | F431/M413: DCU |
| [dcu_v2](../data/registers/dcu_v2.yaml) | A460/F451/F452/F460: DCU1, DCU2, DCU3, DCU4 |
| [dcu_v3](../data/registers/dcu_v3.yaml) | A472/F472: DCU1, DCU2, DCU3 |
| [dcu_v4](../data/registers/dcu_v4.yaml) | A4A0/F467/F4A0/F4A2: DCU1, DCU2, DCU3, DCU4 |
| [dcu_v5](../data/registers/dcu_v5.yaml) | A4A0/F467/F4A0/F4A2: DCU5, DCU6, DCU7, DCU8；A4A8/F4A8: DCU5 |
| [dcu_v6](../data/registers/dcu_v6.yaml) | A4A8/F4A8: DCU1, DCU2, DCU3, DCU4 |
| [dcu_v7](../data/registers/dcu_v7.yaml) | A4A8/F4A8: DCU6 |
| [dcu_v8](../data/registers/dcu_v8.yaml) | A4A8/F4A8: DCU7 |
| [dcu_v9](../data/registers/dcu_v9.yaml) | A4A8/F4A8: DCU8 |
| [dfm_v2](../data/registers/dfm_v2.yaml) | M458: DFM |
| [dmc_common](../data/registers/dmc_common.yaml) | A4A0/A4A8/F467/F4A0/F4A2/F4A8: DMC |
| [dvp_common](../data/registers/dvp_common.yaml) | A4A0/A4A8/F467/F4A0/F4A2/F4A8: DVP |
| [ermu_common](../data/registers/ermu_common.yaml) | A4A8/F4A8: ERMU |
| [eth_v1](../data/registers/eth_v1.yaml) | A4A0/F467/F4A0/F4A2: ETH |
| [eth_v2](../data/registers/eth_v2.yaml) | A4A8/F4A8: ETH |
| [fmac_common](../data/registers/fmac_common.yaml) | A472/A4A0/A4A8/F467/F472/F4A0/F4A2/F4A8: FMAC1, FMAC2, FMAC3, FMAC4 |
| [hall_common](../data/registers/hall_common.yaml) | F431/M413: HALL |
| [hash_v1](../data/registers/hash_v1.yaml) | A448/A460/F448/F451/F452/F460: HASH |
| [hash_v2](../data/registers/hash_v2.yaml) | A472/A4A0/F467/F472/F4A0/F4A2: HASH |
| [hash_v3](../data/registers/hash_v3.yaml) | A4A8/F4A8: HASH |
| [hrpwm_v1](../data/registers/hrpwm_v1.yaml) | A4A0/F4A0/F4A2: HRPWM |
| [hrpwm_v2](../data/registers/hrpwm_v2.yaml) | F334/F336: HRPWM1 |
| [hrpwm_v3](../data/registers/hrpwm_v3.yaml) | F334/F336: HRPWM2, HRPWM3, HRPWM4, HRPWM5, HRPWM6 |
| [hrpwm_v4](../data/registers/hrpwm_v4.yaml) | F334/F336: HRPWM_COMMON |
| [keyscan_common](../data/registers/keyscan_common.yaml) | A448/A460/A472/A4A0/A4A8/F448/F451/F452/F460/F467/F472/F4A0/F4A2/F4A8/M441: KEYSCAN |
| [mau_v1](../data/registers/mau_v1.yaml) | A472/A4A0/F467/F472/F4A0/F4A2: MAU |
| [mau_v2](../data/registers/mau_v2.yaml) | A4A8/F4A8: MAU |
| [mau_v3](../data/registers/mau_v3.yaml) | F431/M413: MAU |
| [mau_v4](../data/registers/mau_v4.yaml) | M458: MAU |
| [mdio_common](../data/registers/mdio_common.yaml) | A472/F472: MDIO |
| [mpu_v1](../data/registers/mpu_v1.yaml) | A448/F448: MPU |
| [mpu_v2](../data/registers/mpu_v2.yaml) | A460/F451/F452/F460: MPU |
| [mpu_v3](../data/registers/mpu_v3.yaml) | A472/F472: MPU |
| [mpu_v4](../data/registers/mpu_v4.yaml) | A4A0/F467/F4A0/F4A2: MPU |
| [mpu_v5](../data/registers/mpu_v5.yaml) | A4A8/F4A8: MPU |
| [mpu_v6](../data/registers/mpu_v6.yaml) | F334/F336: MPU |
| [mpu_v7](../data/registers/mpu_v7.yaml) | M441: MPU |
| [nfc_v1](../data/registers/nfc_v1.yaml) | A4A0/F467/F4A0/F4A2: NFC |
| [nfc_v2](../data/registers/nfc_v2.yaml) | A4A8/F4A8: NFC |
| [ots_v1](../data/registers/ots_v1.yaml) | A460/F451/F452/F460: OTS |
| [ots_v2](../data/registers/ots_v2.yaml) | A472/A4A0/A4A8/F467/F472/F4A0/F4A2/F4A8: OTS |
| [pla_v1](../data/registers/pla_v1.yaml) | A472/F472: PLA |
| [pla_v2](../data/registers/pla_v2.yaml) | F334/F336: PLA |
| [qspi_v1](../data/registers/qspi_v1.yaml) | A448/F448: QSPI |
| [qspi_v2](../data/registers/qspi_v2.yaml) | A460/A4A0/F451/F452/F460/F467/F4A0/F4A2: QSPI |
| [qspi_v3](../data/registers/qspi_v3.yaml) | A472/F472: QSPI |
| [qspi_v4](../data/registers/qspi_v4.yaml) | A4A8/F4A8: QSPI |
| [sdioc_common](../data/registers/sdioc_common.yaml) | A460/A4A0/A4A8/F451/F452/F460/F467/F4A0/F4A2/F4A8: SDIOC1, SDIOC2 |
| [ske_common](../data/registers/ske_common.yaml) | A4A8/F4A8: SKE |
| [smc_v1](../data/registers/smc_v1.yaml) | A448/F448: SMC |
| [smc_v2](../data/registers/smc_v2.yaml) | A472/F472: SMC |
| [smc_v3](../data/registers/smc_v3.yaml) | A4A0/F467/F4A0/F4A2: SMC |
| [smc_v4](../data/registers/smc_v4.yaml) | A4A8/F4A8: SMC |
| [tmr_v10](../data/registers/tmr_v10.yaml) | A460/F451/F452/F460: TMR61, TMR62, TMR63 |
| [tmr_v11](../data/registers/tmr_v11.yaml) | A460/F451/F452/F460: TMR6_COMMON |
| [tmr_v15](../data/registers/tmr_v15.yaml) | A472/F472: TMR61, TMR610, TMR62, TMR63, TMR64, TMR65, TMR66, TMR67, TMR68, TMR69 |
| [tmr_v16](../data/registers/tmr_v16.yaml) | A472/F472: TMR6_COMMON |
| [tmr_v19](../data/registers/tmr_v19.yaml) | A4A0/F4A0/F4A2: TMR61, TMR62, TMR63, TMR64, TMR65, TMR66, TMR67, TMR68；F467: TMR61, TMR62, TMR63 |
| [tmr_v20](../data/registers/tmr_v20.yaml) | A4A0/A4A8/F4A0/F4A2/F4A8: TMR6_COMMON |
| [tmr_v24](../data/registers/tmr_v24.yaml) | A4A8/F4A8: TMR61, TMR62, TMR63, TMR64 |
| [tmr_v25](../data/registers/tmr_v25.yaml) | A4A8/F4A8: TMR65, TMR67 |
| [tmr_v26](../data/registers/tmr_v26.yaml) | A4A8/F4A8: TMR66, TMR68 |
| [tmr_v30](../data/registers/tmr_v30.yaml) | F334/F336: TMR61, TMR62, TMR63, TMR64 |
| [tmr_v31](../data/registers/tmr_v31.yaml) | F334/F336: TMR6_COMMON |
| [tmr_v34](../data/registers/tmr_v34.yaml) | F467: TMR6_COMMON |
| [tmr_v5](../data/registers/tmr_v5.yaml) | A448/F448/M441: TMR61, TMR62 |
| [tmr_v6](../data/registers/tmr_v6.yaml) | A448/F448/M441: TMR6_COMMON |
| [usbfs_v1](../data/registers/usbfs_v1.yaml) | A460/F451/F452/F460: USBFS |
| [usbfs_v2](../data/registers/usbfs_v2.yaml) | A472/F472: USBFS |
| [usbfs_v3](../data/registers/usbfs_v3.yaml) | A4A0/A4A8/F467/F4A0/F4A2/F4A8: USBFS |
| [usbhs_common](../data/registers/usbhs_common.yaml) | A4A0/A4A8/F467/F4A0/F4A2/F4A8: USBHS |
| [vref_common](../data/registers/vref_common.yaml) | A472/F472: VREF |
<!-- peripheral-versions:end -->
