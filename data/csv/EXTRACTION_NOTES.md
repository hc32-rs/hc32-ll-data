# LL 侧 CSV 提取说明（tools/extract_ll_csv.py 自动生成）

来源：`xh_ll_sdk/` 下 27 个 SDK 目录（设备头、hc32_ll_driver、SVD、DS/RM PDF）。
输出：本目录 5 个 CSV，列头见 `tools/CSV_SCHEMA.md`。可重复运行：
`python3 tools/extract_ll_csv.py [--sdk-root X] [--out Y] [--series ...]`

## 各系列行数统计

| 系列 | chips | interrupts | dma | peripherals | SVD 外设数 | FCG 匹配 | 备注 |
|---|---:|---:|---:|---:|---:|---:|---|
| HC32A448 | 3 | 171 | 468 | 68 | 68 | 44 |  |
| HC32A460 | 1 | 153 | 390 | 74 | 74 | 49 |  |
| HC32A472 | 2 | 223 | 602 | 98 | 98 | 70 |  |
| HC32A4A0 | 2 | 153 | 732 | 123 | 123 | 95 |  |
| HC32A4A8 | 6 | 153 | 784 | 134 | 134 | 100 |  |
| HC32F115 | 2 | 37 | 0 | 29 | 29 | 20 |  |
| HC32F120 | 1 | 37 | 0 | 28 | 28 | 19 |  |
| HC32F155 | 8 | 37 | 65 | 33 | 33 | 23 |  |
| HC32F175 | 4 | 36 | 22 | 42 | 42 | 31 |  |
| HC32F334 | 8 | 186 | 252 | 66 | 66 | 38 |  |
| HC32F336 | 2 | 186 | 252 | 66 | 66 | 38 |  |
| HC32F431 | 3 | 83 | 116 | 39 | 39 | 22 |  |
| HC32F448 | 10 | 171 | 468 | 68 | 68 | 44 |  |
| HC32F451 | 3 | 153 | 390 | 73 | 73 | 48 |  |
| HC32F452 | 3 | 153 | 390 | 74 | 74 | 49 |  |
| HC32F460 | 9 | 153 | 390 | 74 | 74 | 49 |  |
| HC32F467 | 3 | 153 | 534 | 102 | 102 | 76 |  |
| HC32F472 | 8 | 223 | 602 | 98 | 98 | 70 |  |
| HC32F4A0 | 10 | 153 | 732 | 123 | 123 | 95 |  |
| HC32F4A2 | 6 | 153 | 732 | 123 | 123 | 95 |  |
| HC32F4A8 | 5 | 153 | 784 | 134 | 134 | 100 |  |
| HC32K116 | 3 | 36 | 100 | 36 | 36 | 22 |  |
| HC32K118 | 3 | 36 | 100 | 36 | 36 | 22 |  |
| HC32M120 | 1 | 37 | 56 | 31 | 31 | 19 |  |
| HC32M413 | 3 | 83 | 116 | 39 | 39 | 22 |  |
| HC32M441 | 4 | 165 | 462 | 61 | 61 | 37 |  |
| HC32M458 | 2 | 120 | 136 | 44 | 44 | 24 |  |

## DMA 编码语义结论

**LL SDK 与旧 DDL 的 DMA_SRC_*（DSL/SPL 高低 4 位编码）完全不同**：所有 LL 系列
的 `hc32_ll_dma.h` 只有 `DMA_SRC_ADDR_FIX/INC/DEC` 之类的通道控制宏，没有外设
请求宏。LL 的 DMA 触发源走 AOS（外设事件路由）：

- 触发源 = 设备头 `EVT_SRC_*` 枚举值（9 位事件编号，0x000~0x1FF），写入
  `AOS->DMAx_TRGSELy`（多单元系列）或 `AOS->DMA_TRGSELy`（单单元系列）；
- 任意 DMA 请求可路由到任意通道（F460 RM 第 11 章 AOS、第 14 章 DMA 证实，
  TRGSEL 复位值 0x1FF，字段 TRGSEL[8:0]）；
- 因此 dma.csv 每行 `channel` 填 RM 原始通道范围（如 `0-3`、`0-7`），一行一个
  外设请求信号，`request` 为 `EVT_SRC_` 前缀去掉后的信号名；
- `trigger_id` 列 = 写入 TRGSEL 的事件编号数值（十进制）：AOS 系列为
  `EVT_SRC_*` 枚举值；F175 为 `DMA_<信号>_REQ` 去移位后的原始请求编号
  （0x40~0x55，即十进制 64~85，写通道 CONFA0.TRISEL）。
- 排除了 RM 表 10-2 中"不可选择为内部触发源"的事件：`*_ERR`、`*_PEERR`、
  `*_COLERR`、软件中断 `*_SWI_IRQ`；DMA 自身 TC/BTC 事件按 RM 可作触发源，保留；
- 通道数取自 `hc32_ll_aos.h` 的 `AOS_DMA<x>_<n>` 宏（每单元 4 或 8 通道），
  DMA 实例名取自 SVD；F115/F120 无 DMA 外设，dma 行数为 0。
- 例外（F175，旧式外设集）：无 AOS/EVT_SRC，DMAC 用固定请求编号——
  `DMA_<信号>_REQ = (0xNN << DMAC_CONFA0_TRISEL_POS)`（0x40~0x55），写入各通道
  CONFA0.TRISEL 选择请求源，4 个通道（CH0~3）仍为自由路由，channel 填 `0-3`。

## 其它假设与说明

- interrupts.csv：以设备头 `IRQn_Type` 为准。共享向量系列（F460/F45x/A460/F448/
  A448/F467/F4A0/F4A2/F4A8/A4A8/A4A0/M441 及 M0+ 的 INTxxx 段）的向量名 INTnnn
  不对应具体外设，description 统一标注"共享中断向量"；外设中断源→向量的路由是
  运行时经 INTC INT_SEL/INT_VSSEL 配置的，无法静态给出，peripherals.csv 的
  interrupts 列对这些系列留空。SVD 中断节点数>0 的系列（F334/F336/F431/M413/
  K116/K118/M458/F472/A472）用 SVD 补充 description，peripherals 的 interrupts
  列填 SVD 外设节点下的中断源名。
- peripherals.csv：地址取自 `hc32-ll-data/svd/*.svd`（派生外设继承 baseAddress）；
  FCG 门控取自 `hc32_ll_fcg.h` 的 `FCG<n>_PERIPH_<实例>` 宏映射到的位宏
  （`PWC_FCG*_*` / `CMU_FCG_*` / F175 的 `SYSCTRL_APBEN*_*`），fcg_reg 填 FCG0~3，
  fcg_field 填实际位宏名。GPIO/CMU/RMU 等常开模块无门控，留空。
- memory.csv：flash_base 取 GCC/IAR 链接脚本 FLASH 区起点（均为 0x00000000）；
  sram_base 取设备头 `SRAM_BASE`，缺省回退链接脚本 RAM 区；sram_hi_base 在主 RAM
  区跨 0x20000000 别名区时填 0x20000000（SRAMH 之后），独立保留区记入 notes。
- chips.csv：解析 DS PDF 订购信息/型号功能对比表。F120、M120 的 SDK 无 DS
  （仅 RM，RM 无型号表），各留一行占位并在 other 标注。HC32F155 订购页版式打散，
  只按两行配对恢复出型号名+封装+主频回退，其余属性列为空。F472/A472/F467/F4A2/
  F4A8/A4A0 等新版式标签与值分离，用"孤儿值行 + 语义验证器"恢复大部分列，
  仍有零星缺口（如 F467 的 I2C/Timer、F4A8 的 GPIO/ADC、A472 的 SRAM/Flash）。
  part_no 一律去掉 `-封装` 后缀；同一 die 的 LQFP/LQG（环保）变体按 part_no 去重。
  HC32A460 订购表为单列合并版式，CAN/SDIO/USB/DMA 列的数值疑似伪影（SVD 实际为
  CAN×1、SDIOC×2、USBFS×1、DMA×2），以 SVD 为准。
- kind 列：实例名去数字后缀小写，TMR01→tmr0、TMR41→tmr4、TMR61→tmr6 等
  Timer 家族保留序号。

## 校验

- dma.csv：(series, dma, request) 无重复；channel 为 `a-b` 范围且合法；
- peripherals.csv 行数应等于 SVD 顶层 peripheral 数（含派生）；
- interrupts 行数 = 设备头 IRQn_Type 枚举项数（含内核负向量）。

## 生成时间

2026-11-14 02:02:33
