# DMA

| 版本 | 系列 | 布局与通道 |
|---|---|---|
| `dma_v3` | F175 | 1×4；CONF + 每通道 CONFA/CONFB/SRCADR/DSTADR，`DMAC_IRQn=7` |
| `dma_v4` | M120 | 1×2；无 BASE1_LLP/BASE2_LLP |
| `dma_v4_1` | F155、K116/K118 | 分别为 1×2、1×4；含 BASE1_LLP/BASE2_LLP |
| `dma_v6` | A460/F451/F452/F460 | 2×4；无 CHENCLR |
| `dma_v6_1` | F334/F336、F431/M413、M441、M458 | 分别为 1×8、1×4、2×6、1×2；有 CHENCLR，无 HPROT |
| `dma_v6_2` | A448/F448、F467/A472/F472/A4A0/F4A0/F4A2/A4A8/F4A8 | 分别为 2×6、2×8；有 CHENCLR 和 CHCTL.HPROT |

`dma_v4_1` 相对 `dma_v4` 增加 BASE1_LLP、BASE2_LLP 和每通道 LLPSEL，CHENCLR 偏移也不同。
M4 布局以 `dma_v6` 为基础，`dma_v6_1` 增加 CHENCLR，`dma_v6_2` 再增加 CHCTL.HPROT。各档的控制器和通道数量按系列登记。

F115/F120 无 DMA。F175 的控制器基址为 0x40024800，通过 `hc32_ll_dmac.h` 配置
CONFA 触发选择。F155/K116/K118/M120 为 0x40013000。M4 的控制器基址为 0x40053000，
第二控制器在 0x40053400，触发经 AOS 配置。M4 通道组步长为 0x40，链表指针使用
每通道的 LLP 寄存器。寄存器位宽需单独核对，F460 的 `DMA_SWREQ_SWREQ`
在设备头和 SVD 中均为 8 位掩码（0xFF），即使可用通道数为四也应保留该定义。

F155/M120 的 DMA 中断源通过 INTC 签入通用槽；K116/K118 的设备头有固定
`DMA_IRQn=4`。M4 中断按[设备头向量分组](gpio-int.md)处理，匿名槽系列的
`IRQn_Type` 为 INT000–143。
