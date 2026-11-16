# SVD 修改日志（hc32-ll-data）

`svd/` 下的 SVD 允许修改，但每次修改前必须在此追加一行记录
（规则见工作区根目录 AGENTS.md）：

| 日期 | 文件 | 修改内容 | 原因 |
|------|------|----------|------|
| 2026-11-14 | 全部 27 个 SVD | 行尾统一为 LF，寄存器内容不变 | 入库归一化（见首个归档提交） |
| 2026-11-16 | 9 个 SVD（HC32A472/F334/F336/F431/F472/K116/K118/M413/M458） | 按各系列 data/family 映射替换外设 `<interrupt>` 节点（旧值不可信，删除后按 data/interrupts/*.yaml 的设备头 IRQn 编号重注入，共 430 个；中断名与设备头拼写不一致的条目跳过，对应外设旧节点保留；删而无替 143 个、未解析名 517 条，明细见 data/csv/interrupt_injection_report.txt） | 为 metapac 生成提供完备中断数据源（hc32-data-tools/patch_svd_interrupts.py）；其余 18 个系列（HC32A448/A460/A4A0/A4A8、F115/F120/F155/F175/F448/F451/F452/F460/F467/F4A0/F4A2/F4A8、M120/M441）为共享 INTxxx 重定向向量模型（外设事件运行时经 INTC/AOS 路由，无外设专属 NVIC 向量，family 无 interrupts 段），整系列跳过不修改 |
| 2026-11-16 | 全部 27 个 SVD | 行尾改由 .gitattributes 统一管控：仓库内归一 LF、检出统一 CRLF（原 `* -text` 策略废止；本仓库 SVD 检出从 LF 变为 CRLF） | 全仓库行尾统一（见 .gitattributes） |
