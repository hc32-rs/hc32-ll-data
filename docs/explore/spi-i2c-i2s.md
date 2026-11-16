# SPI、I2C 与 I2S

SPI 先区分 F175 的 CR0/CR1/HDOE/SSI 与 CFG1/CFG2 架构，后者的 CR/CR1、MBR 位宽、
FMIDLF/RXMDS 差异用后缀保留。20 份 M4 SVD 的 SPI.DR 都无位域；
F460 设备头用 __IO uint32_t DR，驱动以 READ_REG32/WRITE_REG32 访问整寄存器。

I2C 从 F175 的 BRREN/BRR/DR、多 CR 配置/滤波、超时 SLTR 到 FSTR/从地址扩展排序。
K116/K118 的 SLTR.TOUTLOW/TOUTHIGH 位于 bit 30/31，F460 各占 16 位，必须分布局。
M458 的 SLVADRR 拼写保留为后缀差异，不手改生成源。
I2S 使用独立 i2s 名称，CTRL/SR/ER/CFGR/TXBUF/RXBUF/PR 的位域差异各自保留。
当前源 kind=i 仅作为迁移来源，不再用于目标外设名。

设备头与驱动位于各 SDK 的 drivers/cmsis/Device/*/*/Include 和 drivers/hc32_ll_driver。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下为待确认命名，当前来源仍使用旧文件名。系列名前缀 `HC32` 省略。

### i2c：v1

旧式地址匹配 → CR 配置/滤波 → 超时 SLTR → FSTR/从地址扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ll_v1.rs` | [i_v12](../../data/registers/i_v12.yaml) | F175: I2C0, I2C1 | 9 / 17 | 本档位对照基准；完整定义见来源 YAML |

### i2c：v2

旧式地址匹配 → CR 配置/滤波 → 超时 SLTR → FSTR/从地址扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ll_v2.rs` | [i_v11](../../data/registers/i_v11.yaml) | F155: I2C | 11 / 67 | 本档位对照基准；完整定义见来源 YAML |
| `i2c_ll_v2_1.rs` | [i_v9](../../data/registers/i_v9.yaml) | F115: I2C | 11 / 69 | 相对 i2c_ll_v2：定义差异 DRR/DTR |
| `i2c_ll_v2_2.rs` | [i_v10](../../data/registers/i_v10.yaml) | F120/M120: I2C | 11 / 70 | 相对 i2c_ll_v2：定义差异 DRR/DTR/FLTR |

### i2c：v3

旧式地址匹配 → CR 配置/滤波 → 超时 SLTR → FSTR/从地址扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ll_v3.rs` | [i_v15](../../data/registers/i_v15.yaml) | K116/K118: I2C | 13 / 77 | 本档位对照基准；完整定义见来源 YAML |
| `i2c_ll_v3_1.rs` | [i_v2](../../data/registers/i_v2.yaml) | A460/F451/F452/F460/F467: I2C1, I2C2, I2C3；A4A0/F4A0/F4A2: I2C1, I2C2, I2C3, I2C4, I2C5, I2C6 | 13 / 78 | 相对 i2c_ll_v3：定义差异 CLR/CR4/DRR/DTR/SLTR |

### i2c：v4

旧式地址匹配 → CR 配置/滤波 → 超时 SLTR → FSTR/从地址扩展。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2c_ll_v4.rs` | [i_v6](../../data/registers/i_v6.yaml) | A4A8/F4A8: I2C1, I2C2, I2C3, I2C4, I2C5, I2C6 | 15 / 95 | 本档位对照基准；完整定义见来源 YAML |
| `i2c_ll_v4_1.rs` | [i_v1](../../data/registers/i_v1.yaml) | A448/F448/M441: I2C1, I2C2 | 15 / 96 | 相对 i2c_ll_v4：定义差异 CR4/SLR0/SLR1 |
| `i2c_ll_v4_2.rs` | [i_v13](../../data/registers/i_v13.yaml) | F334/F336: I2C | 15 / 97 | 相对 i2c_ll_v4：定义差异 CCR/CR4/SLR0/SLR1 |
| `i2c_ll_v4_3.rs` | [i_v14](../../data/registers/i_v14.yaml) | F431/M413: I2C | 15 / 97 | 相对 i2c_ll_v4：定义差异 CR4/SLR0/SLR1 |
| `i2c_ll_v4_4.rs` | [i_v16](../../data/registers/i_v16.yaml) | M458: I2C | 15 / 97 | 相对 i2c_ll_v4：增加 SLVADRR；缺少 SLVADDR；定义差异 CR4/SLR0/SLR1 |
| `i2c_ll_v4_5.rs` | [i_v4](../../data/registers/i_v4.yaml) | A472/F472: I2C1, I2C2, I2C3 | 15 / 97 | 相对 i2c_ll_v4：定义差异 CCR/CR4/SLR0/SLR1 |

### i2s：v1

音频串行控制；位域分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `i2s_ll_v1.rs` | [i_v3](../../data/registers/i_v3.yaml) | A460/F451/F452/F460: I2S1, I2S2, I2S3, I2S4 | 7 / 30 | 本档位对照基准；完整定义见来源 YAML |
| `i2s_ll_v1_1.rs` | [i_v5](../../data/registers/i_v5.yaml) | A4A0/F467/F4A0/F4A2: I2S1, I2S2, I2S3, I2S4 | 7 / 31 | 相对 i2s_ll_v1：定义差异 CTRL |
| `i2s_ll_v1_2.rs` | [i_v8](../../data/registers/i_v8.yaml) | A4A8/F4A8: I2S2, I2S3, I2S4 | 7 / 32 | 相对 i2s_ll_v1：定义差异 CTRL/SR |
| `i2s_ll_v1_3.rs` | [i_v7](../../data/registers/i_v7.yaml) | A4A8/F4A8: I2S1 | 7 / 33 | 相对 i2s_ll_v1：定义差异 CTRL/SR |

### spi：v1

旧式 CR0/CR1、HDOE/SSI。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `spi_ll_v1.rs` | [spi_v5](../../data/registers/spi_v5.yaml) | F175: SPI0, SPI1 | 7 / 41 | 本档位对照基准；完整定义见来源 YAML |

### spi：v2

CFG1/CFG2 配置架构；CR/CR1、分频和接收控制分变体。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `spi_ll_v2.rs` | [spi_v4](../../data/registers/spi_v4.yaml) | F115/F120/F155/M120: SPI | 5 / 28 | 本档位对照基准；完整定义见来源 YAML |
| `spi_ll_v2_1.rs` | [spi_v7](../../data/registers/spi_v7.yaml) | K116/K118: SPI1, SPI2 | 5 / 34 | 相对 spi_ll_v2：增加 CR；缺少 CR1；定义差异 CFG1/CFG2/SR |
| `spi_ll_v2_2.rs` | [spi_v2](../../data/registers/spi_v2.yaml) | A460/F451/F452/F460: SPI1, SPI2, SPI3, SPI4；A4A0/F467/F4A0/F4A2: SPI1, SPI2, SPI3, SPI4, SPI5, SPI6 | 5 / 40 | 相对 spi_ll_v2：定义差异 CFG1/CFG2/CR1/DR |
| `spi_ll_v2_3.rs` | [spi_v1](../../data/registers/spi_v1.yaml) | A448/F448/M441: SPI1, SPI2, SPI3；A472/F472: SPI1, SPI2, SPI3, SPI4 | 5 / 42 | 相对 spi_ll_v2：增加 CR；缺少 CR1；定义差异 CFG1/CFG2/DR |
| `spi_ll_v2_4.rs` | [spi_v3](../../data/registers/spi_v3.yaml) | A4A8/F4A8: SPI1, SPI2, SPI3, SPI4, SPI5, SPI6 | 5 / 43 | 相对 spi_ll_v2：增加 CR；缺少 CR1；定义差异 CFG1/CFG2/DR/SR |
| `spi_ll_v2_5.rs` | [spi_v6](../../data/registers/spi_v6.yaml) | F334/F336/F431/M413: SPI | 5 / 43 | 相对 spi_ll_v2：增加 CR；缺少 CR1；定义差异 CFG1/CFG2/DR/SR |
| `spi_ll_v2_6.rs` | [spi_v8](../../data/registers/spi_v8.yaml) | M458: SPI1, SPI2 | 5 / 44 | 相对 spi_ll_v2：增加 CR；缺少 CR1；定义差异 CFG1/CFG2/DR/SR |
<!-- peripheral-versions:end -->
