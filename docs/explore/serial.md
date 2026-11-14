# LPUART、USART 与 LIN

## Cortex-M0+

| 版本 | 系列与实例 |
|---|---|
| `lpuart_v6`、`usart_v2` | F175 的 LPUART0/1、USART0–5；USART 为 CR1 起始的旧式 14 寄存器块 |
| `usart_v3` | F115/F120/F155/M120 全部 USART |
| `usart_v4` | K116/K118 的 USART1/2 |
| `usart_v5` | K116/K118 的 USART3/4 |

## Cortex-M4

| 文件 | 覆盖（系列:实例） | 特征 |
|---|---|---|
| usart_v6.yaml | A460,F451,F452,F460：全部 | 简化 RTO+SC |
| usart_v7.yaml | A4A0,F467,F4A0,F4A2：U1,2,6,7 | v6+HDSEL/RTSE |
| usart_v8.yaml | 同上：U3,4,8,9（F467 只有 U3,4） | SC+HDSEL/RTSE（无 简化 RTO） |
| usart_v9.yaml | 同上：U5,10 | HDSEL/RTSE+LIN/LBMC+LBMC |
| usart_v10.yaml | A4A8,F4A8：U1,2,6,7 | v7+TEND+RS485 |
| usart_v11.yaml | 同上：U3,4,8,9 | v8+TEND+RS485+CRTOF/RTOF |
| usart_v12.yaml | 同上：U5,10 | v9+TEND+RTO(部分)+RS485+DIV |
| usart_v13.yaml | A472,F472：U1,2,4,5 | 简化 RTO+SC+TEND(无ULBREN)+HDSEL/RTSE |
| usart_v14.yaml | 同上：U3,6 | TEND(无ULBREN)+HDSEL/RTSE+LIN/LBMC+DIV |
| usart_v15.yaml | A448,F448：U1,2,4,5 | 简化 RTO+SC+TEND+HDSEL/RTSE |
| usart_v16.yaml | 同上：U3,6 | TEND+HDSEL/RTSE+LIN/LBMC+DIV |
| usart_v17.yaml | F334,F336：U1 | v15+RS485（命名方言 CR_TENDIE/TENDF，记别名） |
| usart_v18.yaml | 同上：U2-4 | LIN/LBMC+RTO(部分)+DIV+RS485+TEND |
| usart_v19.yaml | F431,M413：U1 | =v4+TDS |
| usart_v20.yaml | 同上：U2-4 | =v5+TDS |
| usart_v21.yaml | M441：U1,2,4,5 | 简化 RTO+DIV+TEND+HDSEL/RTSE |
| usart_v22.yaml | 同上：U3,6 | TEND+DIV+HDSEL/RTSE |
| usart_v23.yaml | M458：全部 | =v5+ABR+TDS |

归档 SVD 解析派生实例后得到 M4 的 18 个 USART 位域签名。LL 驱动名统一为
`hc32_ll_usart.h`，LIN 能力按实例检查 `LINEN` 位和相应驱动选项。
A460/F451/F452/F460 与 M441 的 SVD USART 无 LINEN。中断建模见[IRQ 向量](gpio-int.md)。
