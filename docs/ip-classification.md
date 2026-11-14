# HC32 LL 外设 IP 分类

覆盖 7 个 Cortex-M0+ 系列和 20 个 Cortex-M4 系列。HC32F175 的设备头引用 Cortex-M0+ 内核头。它使用 LL 接口，多个外设沿用旧式寄存器布局。

[布局编号与功能档位](layout-versions.md)列出已重排的分组和依据。生成寄存器数据前需比较寄存器偏移、
访问属性和位域；基址、中断、时钟门控与通道数按系列或实例记录。[SDK 核验记录](sdk-verification.md)列出已核查范围。

| 外设组 | 本仓库分类 |
|---|---|
| [模拟](explore/analog.md) | F175 为 `adc_v5a`，其余 Cortex-M0+ 的 ADC 归 `adc_v9` 候选。M4 有 `adc_v10`、`dac_v3` 与 `cmp_v3–v7` 家族。F155/K116/K118/M120 例程使用 ADC 完成事件触发 DMA。 |
| [时钟、复位与校准](explore/clock-reset.md) | F175 为 sysctrl_v13、ctrim_v5、csu_v1；其余六个 Cortex-M0+ 系列分为 cmu_v1–v5、reset_v9/v10、ctc_v1–v3、icg_v1–v6。M4 续编 CMU/RMU/ICG，部分 CTC 可沿用。FCM 按 SVD 和设备头分组后定版。 |
| [电源、低功耗与看门狗](explore/power-wdt.md) | F175 使用 lvd_v1（LOCK 差异）、iwdt_v2、wwdt_v2。其余六个 Cortex-M0+ 系列的电源块为 pwc_v1–v3，看门狗为 swdt_v1_1。M4 扩展 `pwc_v4–v14`，A460/F451/F452/F460 的 SWDT 为无 CR 的 `swdt_v1`。 |
| [GPIO、中断与调试](explore/gpio-int.md) | F175 为 AFR 式 gpio_v4、exti_v1 和 dbg_v3；其他 Cortex-M0+ 为 gpio_v5/v6、intc_v1–v3、dbg_v2，部分另有 event_port_v1/v2 或 dbgt_v1/v2。M4 GPIO 有字节和 16 位端口两类，IRQ 按通用槽、混合、固定具名三类建模。 |
| [LPUART、USART 与 LIN](explore/serial.md) | F175 为 lpuart_v6/usart_v2。F115/F120/F155/M120 为 usart_v3；K116/K118 按实例分 usart_v4/v5。M4 USART 建议 usart_v6–v23，按实例登记。 |
| [SPI、I2C 与 I2S](explore/spi-i2c-i2s.md) | F175 为 spi_v2/i2c_v3；其余 Cortex-M0+ 为 spi_v4/v5、i2c_v4/v5。M4 的 SPI 分 `spi_v6/v7`，I2C 分 13/15 寄存器两组，I2S 分 4 个位域簇。 |
| [高级定时器与紧急刹车](explore/timer-adv.md) | F175 ATIM3 为单寄存器块，建议 `atim_v3f`。M120、K116/K118 的 EMB 为 emb_v1/v2，M4 增 emb_v3–v11。 |
| [通用、基本与低功耗定时器](explore/timer-gen.md) | F175 为 gtim_v2、lptim_v5。其余 Cortex-M0+ 涉及 TMR0/TMRB/TMR2/TMR4/TMRA；M4 续编 TMR0、TMR2、TMR4、TMRA；A472/F472 的 TMR4 可沿用 `tmr4_v1`。 |
| [异步事件路由 AOS](explore/pca-bt-pcnt-aos.md) | AOS 以基础路由为 `aos_v1`，再按 DMA、事件端口和路由目标规模分档。F175 无 AOS。 |
| [DMA](explore/dma.md) | F175 使用 hc32_ll_dmac.h，旧 CONF/CONFA 布局归 dma_v3；M0+ 的 EN/SAR 布局归 `dma_v4` 族，M4 布局归 `dma_v6` 族。F115/F120 无 DMA。 |
| [Flash、RAM 与 SRAM](explore/memory.md) | Cortex-M0+ 的 EFM 为 efm_v1–v5，SRAM 控制为 sram_v1/v2；M4 为 efm_v6–v14、sram_v3–v9。F175 的 EFM 与 SRAM 控制各有单独布局。 |
| [CRC、AES、TRNG、RTC 与 CAN](explore/crypto-comm.md) | F175 为 crc_v1、rtc_v2 候选变体；其余 Cortex-M0+ 使用 crc_v2，部分含 rtc_v3、mcan_v1 或 FCM。M4 有 CRC/AES/TRNG/CAN/MCAN 增量，按寄存器块与位域拆分。 |
