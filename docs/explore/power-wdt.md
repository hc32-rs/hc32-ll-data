# 电源、低功耗与看门狗

| 版本 | 系列或实例 |
|---|---|
| `lvd_v1`、`iwdt_v2`、`wwdt_v2` | F175，LVD 另有 LOCK 寄存器 |
| `pwc_v1/v2/v3` | 分别为 F115/F120/M120、F155、K116/K118 |
| `swdt_v1` | A460/F451/F452/F460 的 SWDT，只有 SR/RR，配置经 ICG0 |
| `swdt_v1_1` | F115/F120/F155、K116/K118、M120 的 SWDT；M4 全部 20 个 WDT 及其余 16 个 SWDT |

`swdt_v1_1` 的寄存器为 CR@0、SR@4、RR@8。SVD 位域对比复核了两组实例。M4 的 WDT、SWDT
分别在 0x40049000、0x40049400。M4 的低压检测由 PWC.PVD 寄存器和
`hc32_ll_pwc.h` 的 `PWC_LVD_*` API 表达。

M4 PWC 骨架（0x40048000 全功能版）：`FCG0-3@0x0-0xC + FCG0PC@0x10`（时钟门控）、`WKTCR/WKTC0-2`（唤醒定时器）、`PWRC0-6`（电源模式）、`PVDCR0/1/PVDFCR/PVDLCR/PVDICR/PVDDSR`（PVD1+PVD2 双路电压检测）、`PDWKE0-2/PDWKES/PDWKF0-1`（掉电唤醒源）、`RAMPC0/RAMOPM/PRAMLPC`（RAM 保持）、`STPMCR`、`FPRC`、`DBGC`、可选 `BKR0-127/VBATCR/VBATRSTR`（备份寄存器）。FCG 位图随芯片外设集合变化，是 version 拆分的主因。

| version | 系列 | 基址 | 寄存器数 | 特征 |
|---|---|---|---|---|
| pwc_v4 | F334, F336 | 0x40048000 | 29 | 0xC4xx 窗口；WKTCR@0x4400；含 LPMCSR/LPMCSCR；无 BKR |
| pwc_v5 | F431, M413 | 0x40054000 | 12 | **精简版**：STPMCR/RAMCR/PVD*/FPRC/DBGC/LPMCSR/LPMCSCR；无 FCG（FCG 在 CMU@0x40054400）、无 PWRC/PDWKE |
| pwc_v6 | M458 | 0x40054400 | 11 | = v5 − RAMCR（PVD1LVL 4bit） |
| pwc_v7 | A448, F448 | 0x40048000 | 30 | 0x4C00 窗口（PWRC0-6, PVD@0x4C14, PDWKE@0x4C28）；WKTCR@0x4400；STPMCR@0xC00C；FPRC@0xC3FE |
| pwc_v8 | M441 | 0x40048000 | 30 | 寄存器集同 v7，FCG 位图与 PWRC5/PWRC6 字段不同 |
| pwc_v9 | F451 | 0x40048000 | 29 | 0xC4xx 窗口；+MDSWCR/PWCMR/XTAL32CS；无 PWRC4-6/PRAMLPC；无 LPMCSR |
| pwc_v10 | F452, F460, A460 | 0x40048000 | 29 | 0xC4xx 窗口；含 PWRC0-3/PVD/PDWKE/RAMPC0/RAMOPM；无 LPMCSR |
| pwc_v11 | F467 | 0x40048000 | 160 | 备份域版：BKR×128 + VBATCR/VBATRSTR + WKTC0-2 |
| pwc_v12 | F472, A472 | 0x40048000 | 27 | 0x4C00 窗口；无 PWRC2/PWRC3/RAMOPM；含 LPMCSR |

F431/M413 的 PWC 在 0x40054000、CMU 在 0x40054400，M458 的地址分配对调。
PWC 的备份域与门控位按型号建模。向量号从 `IRQn_Type` 或
`INT_SRC_*` 取得。
