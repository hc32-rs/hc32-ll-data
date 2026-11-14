# SPI、I2C 与 I2S

## Cortex-M0+

| 版本 | 系列 |
|---|---|
| `spi_v2`、`i2c_v3` | F175，SPI 为 CR0/CR1/HDOE/SSI/SR/ICR/DR，I2C 为 BRREN/BRR/CR/DR 起始 |
| `spi_v4`、`i2c_v4` | F115/F120/F155/M120 |
| `spi_v5`、`i2c_v5` | K116/K118 |

## Cortex-M4

| 版本 | 系列 | 寄存器布局 |
|---|---|---|
| `spi_v6` | A460/F451/F452/F460/F467/A4A0/F4A0/F4A2 | DR、CR1、CFG1、SR、CFG2；MBR 4 位 |
| `spi_v7` | A448/F448/M441/A472/F472/F334/F336/F431/M413/A4A8/F4A8/M458 | CR 命名，MBR 3 位；FMIDLF/RXMDS 等差异分系列登记 |
| `i2c_v5` 家族 | A460/F451/F452/F460/F467/A4A0/F4A0/F4A2 | 与 K116/K118 共 13 个同偏移寄存器，字段定义随系列 |
| `i2c_v6` | 其余 12 个 M4 LL 系列 | 上一组布局加 FSTR 与 SLVADDR（M458 命名为 SLVADRR） |
| `i2s_v2` 家族 | A460/F451/F452/F460/F467/A4A0/F4A0/F4A2/A4A8/F4A8 | CTRL、SR、ER、CFGR、TXBUF、RXBUF、PR；按位域分 4 个簇 |

M4 的 20 份 SVD 均把 SPI.DR 描述为无位域的寄存器。F460 的设备头用
`__IO uint32_t DR`，驱动通过 `READ_REG32/WRITE_REG32` 访问整寄存器。
I2C.SLTR 的 TOUTLOW/TOUTHIGH 在 K116/K118 设备头是 bit 30/31，在 F460 各占 16 位。
即使共用 `i2c_v5` 布局，也应分别定义位域。设备头和驱动在各 SDK 的
`drivers/cmsis/Device/*/*/Include/` 和 `drivers/hc32_ll_driver/`。
