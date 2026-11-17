# LPUART、USART 与 LIN

LL USART 按基础串口、复合协议/超时控制、自动波特率或 RS485 等扩展整理。
SmartCard 与 LIN/LBMC 是不同分支，同主档位不表示一个实例具备两者全部功能。
HDSEL/RTSE、TEND、DIV、RTO、TDS 与命名差异均保留实际生成布局，不能把原先的静态签名数当作文件数。
F175 的 CR1 起始 14 寄存器布局为独立分支，包含 TOR/ADDR/IR；其 LPUART 单独登记。

LL 驱动统一为 hc32_ll_usart.h，LIN 按 LINEN 与实例选项检查。
A460/F451/F452/F460 与 M441 的 SVD USART 无 LINEN。
F431/M413 在对应 K116/K118 功能分支上增加 TDS，M458 增加自动波特率相关控制。
A4A8/F4A8、F334/F336 的扩展及各实例的 RTO/SC/LIN 组合独立保留。
中断建模见[IRQ 向量](gpio-int.md)。

[编号规则](../layout-versions.md) · [实例 CSV](../peripheral-versions.csv) · [SDK 核验](../sdk-verification.md)。

<!-- peripheral-versions:begin -->
## 版本与系列对照

以下名称与生成输入保持一致。系列名前缀 `HC32` 省略。

### lpuart：v1

低功耗串口，SCNT/BSEL/MODU 与 SDECON 控制。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `lpuart_ll_v1.rs` | [lpuart_ll_v1](../../data/registers/lpuart_ll_v1.yaml) | F175: LPUART0, LPUART1 | 10 / 42 | 本档位对照基准；完整定义见来源 YAML |

### usart：v1

基础串口与流控；LINEN、半双工等按实例保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `usart_ll_v1.rs` | [usart_ll_v1](../../data/registers/usart_ll_v1.yaml) | F115/F120: USART1, USART2, USART3, USART4；F155: USART1, USART2, USART3, USART4, USART5, USART6；M120: USART1, USART2, USART3 | 8 / 35 | 本档位对照基准；完整定义见来源 YAML |
| `usart_ll_v1_1.rs` | [usart_ll_v1_1](../../data/registers/usart_ll_v1_1.yaml) | M441: USART3, USART6 | 8 / 41 | 相对 usart_ll_v1：定义差异 BRR/CR1/CR2/CR3/PR 等 6 项 |

### usart：v2

复合功能串口：SmartCard、LIN/LBMC 或接收超时，同档不同分支；旧式 CR1 起始布局，TOR/ADDR/IR；与现代复合功能串口同档不同分支。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `usart_ll_v2.rs` | [usart_ll_v2](../../data/registers/usart_ll_v2.yaml) | A4A0/F4A0/F4A2: USART3, USART4, USART8, USART9；F467: USART3, USART4 | 8 / 39 | 本档位对照基准；完整定义见来源 YAML |
| `usart_ll_v2_1.rs` | [usart_ll_v2_1](../../data/registers/usart_ll_v2_1.yaml) | A460/F451/F452/F460: USART1, USART2, USART3, USART4 | 8 / 41 | 相对 usart_ll_v2：定义差异 CR1/CR3/SR |
| `usart_ll_v2_2.rs` | [usart_ll_v2_2](../../data/registers/usart_ll_v2_2.yaml) | A4A0/F467/F4A0/F4A2: USART1, USART2, USART7 | 8 / 43 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_3.rs` | [usart_ll_v2_3](../../data/registers/usart_ll_v2_3.yaml) | A4A0/F467/F4A0/F4A2: USART6 | 8 / 43 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_4.rs` | [usart_ll_v2_4](../../data/registers/usart_ll_v2_4.yaml) | M441: USART1, USART2, USART4, USART5 | 8 / 45 | 相对 usart_ll_v2：定义差异 CR1/CR3/PR/SR |
| `usart_ll_v2_5.rs` | [usart_ll_v2_5](../../data/registers/usart_ll_v2_5.yaml) | A472/F472: USART1 | 8 / 46 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_6.rs` | [usart_ll_v2_6](../../data/registers/usart_ll_v2_6.yaml) | A472/F472: USART2 | 8 / 46 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_7.rs` | [usart_ll_v2_7](../../data/registers/usart_ll_v2_7.yaml) | A472/F472: USART4 | 8 / 46 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_8.rs` | [usart_ll_v2_8](../../data/registers/usart_ll_v2_8.yaml) | A472/F472: USART5 | 8 / 46 | 相对 usart_ll_v2：定义差异 CR1/SR |
| `usart_ll_v2_9.rs` | [usart_ll_v2_9](../../data/registers/usart_ll_v2_9.yaml) | A448/F448: USART1, USART2, USART4, USART5 | 8 / 47 | 相对 usart_ll_v2：定义差异 CR1/PR/SR |
| `usart_ll_v2_10.rs` | [usart_ll_v2_10](../../data/registers/usart_ll_v2_10.yaml) | A4A0/F467/F4A0/F4A2: USART10 | 9 / 54 | 相对 usart_ll_v2：增加 LBMC；定义差异 BRR/CR1/CR2/CR3/PR 等 6 项 |
| `usart_ll_v2_11.rs` | [usart_ll_v2_11](../../data/registers/usart_ll_v2_11.yaml) | A4A0/F467/F4A0/F4A2: USART5 | 9 / 54 | 相对 usart_ll_v2：增加 LBMC；定义差异 BRR/CR1/CR2/CR3/PR 等 6 项 |
| `usart_ll_v2_12.rs` | [usart_ll_v2_12](../../data/registers/usart_ll_v2_12.yaml) | A472/F472: USART3 | 9 / 59 | 相对 usart_ll_v2：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v2_13.rs` | [usart_ll_v2_13](../../data/registers/usart_ll_v2_13.yaml) | A472/F472: USART6 | 9 / 59 | 相对 usart_ll_v2：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v2_14.rs` | [usart_ll_v2_14](../../data/registers/usart_ll_v2_14.yaml) | A448/F448: USART3, USART6 | 9 / 60 | 相对 usart_ll_v2：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v2_15.rs` | [usart_ll_v2_15](../../data/registers/usart_ll_v2_15.yaml) | F175: USART0, USART1, USART2, USART3, USART4, USART5 | 14 / 78 | 相对 usart_ll_v2：增加 ADDMR/ADDR/BDR/ICR/IER 等 7 项；缺少 PR；定义差异 BRR/CR1/CR2/CR3/RDR 等 7 项 |

### usart：v3

串口扩展：RS485 或自动波特率；SmartCard/LIN 分支分别保留。

| 目标文件 | 当前来源 | 系列：实例 | 寄存器 / 字段条目 | 变体差异提示 |
|---|---|---|---:|---|
| `usart_ll_v3.rs` | [usart_ll_v3](../../data/registers/usart_ll_v3.yaml) | A4A8/F4A8: USART3 | 8 / 49 | 本档位对照基准；完整定义见来源 YAML |
| `usart_ll_v3_1.rs` | [usart_ll_v3_1](../../data/registers/usart_ll_v3_1.yaml) | A4A8/F4A8: USART4 | 8 / 49 | 相对 usart_ll_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `usart_ll_v3_2.rs` | [usart_ll_v3_2](../../data/registers/usart_ll_v3_2.yaml) | A4A8/F4A8: USART8 | 8 / 49 | 相对 usart_ll_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `usart_ll_v3_3.rs` | [usart_ll_v3_3](../../data/registers/usart_ll_v3_3.yaml) | A4A8/F4A8: USART9 | 8 / 49 | 相对 usart_ll_v3：寄存器坐标/内联字段未见差异；保留独立 IR 节点/枚举/描述来源，不据此合并 |
| `usart_ll_v3_4.rs` | [usart_ll_v3_4](../../data/registers/usart_ll_v3_4.yaml) | A4A8/F4A8: USART1 | 8 / 51 | 相对 usart_ll_v3：定义差异 CR1 |
| `usart_ll_v3_5.rs` | [usart_ll_v3_5](../../data/registers/usart_ll_v3_5.yaml) | A4A8/F4A8: USART2 | 8 / 51 | 相对 usart_ll_v3：定义差异 CR1 |
| `usart_ll_v3_6.rs` | [usart_ll_v3_6](../../data/registers/usart_ll_v3_6.yaml) | A4A8/F4A8: USART6 | 8 / 51 | 相对 usart_ll_v3：定义差异 CR1 |
| `usart_ll_v3_7.rs` | [usart_ll_v3_7](../../data/registers/usart_ll_v3_7.yaml) | A4A8/F4A8: USART7 | 8 / 51 | 相对 usart_ll_v3：定义差异 CR1 |
| `usart_ll_v3_8.rs` | [usart_ll_v3_8](../../data/registers/usart_ll_v3_8.yaml) | F334/F336: USART1 | 8 / 51 | 相对 usart_ll_v3：定义差异 CR1/RDR/SR |
| `usart_ll_v3_9.rs` | [usart_ll_v3_9](../../data/registers/usart_ll_v3_9.yaml) | K116/K118: USART1, USART2 | 8 / 52 | 相对 usart_ll_v3：定义差异 CR1/PR |
| `usart_ll_v3_10.rs` | [usart_ll_v3_10](../../data/registers/usart_ll_v3_10.yaml) | F431/M413: USART1 | 8 / 53 | 相对 usart_ll_v3：定义差异 CR1/CR3/PR |
| `usart_ll_v3_11.rs` | [usart_ll_v3_11](../../data/registers/usart_ll_v3_11.yaml) | A4A8/F4A8: USART10 | 9 / 67 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v3_12.rs` | [usart_ll_v3_12](../../data/registers/usart_ll_v3_12.yaml) | A4A8/F4A8: USART5 | 9 / 67 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v3_13.rs` | [usart_ll_v3_13](../../data/registers/usart_ll_v3_13.yaml) | F334/F336: USART2, USART3, USART4 | 9 / 67 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/RDR 等 6 项 |
| `usart_ll_v3_14.rs` | [usart_ll_v3_14](../../data/registers/usart_ll_v3_14.yaml) | K116/K118: USART3, USART4 | 9 / 73 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v3_15.rs` | [usart_ll_v3_15](../../data/registers/usart_ll_v3_15.yaml) | F431/M413: USART2, USART3, USART4 | 9 / 74 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
| `usart_ll_v3_16.rs` | [usart_ll_v3_16](../../data/registers/usart_ll_v3_16.yaml) | M458: USART1, USART2, USART3, USART4 | 9 / 85 | 相对 usart_ll_v3：增加 LBMC；定义差异 CR1/CR2/CR3/PR/SR |
<!-- peripheral-versions:end -->
