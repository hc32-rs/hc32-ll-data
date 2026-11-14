# CRC、AES、TRNG、RTC 与 CAN

## Cortex-M0+

| 版本 | 系列 |
|---|---|
| `crc_v1` | F175，CRC@0x40027800，驱动为 `hc32_ll_crc.h` |
| `crc_v2` | F115/F120/F155、K116/K118、M120 |
| `rtc_v2` 子型 | F175 |
| `rtc_v3` 家族 | F115/F155、K116/K118（有 FCLR 差异） |
| `mcan_v1` | K116/K118，报文 RAM ECC 在 MCANRAM |

## Cortex-M4

RTC 在 17 个 M4 LL 系列的 SVD 中有独立节点，按寄存器名、偏移、位域可分四组。

| RTC 组 | 系列 | 顶层寄存器数 |
|---|---|---:|
| 1 | A448/A472/F334/F336/F448/F472/M441 | 16 |
| 2 | A460/F451/F452/F460 | 16 |
| 3 | A4A0/F467/F4A0/F4A2 | 24 |
| 4 | A4A8/F4A8 | 24 |

| 版本 | 系列与特征 |
|---|---|
| `crc_v2` | A448/A472/A4A0/F334/F336/F448/F467/F472/F4A0/F4A2/M441，与 M0+ 组同位域签名 |
| `crc_v3` | A4A8/F4A8/F431/M413/M458，双结果寄存器 |
| `crc_v3_flg` | A460/F451/F452/F460，有独立 FLG |
| `aes_v1` 候选 | A460/F451/F452/F460，AES-128 |
| `aes_v2` 候选 | A448/A472/A4A0/F448/F467/F472/F4A0/F4A2，含 KEYSIZE；AES 数据与密钥寄存器用 DR/KR 命名 |
| `trng_v2` | A448/A460/A472/A4A0/A4A8/F448/F451/F452/F460/F467/F472/F4A0/F4A2/F4A8/M458，M458 有 ENDIE 差异 |
| CAN 23/25/31 寄存器子型 | 分别为 A460/F452/F460、A472/F472/A4A0/F467/F4A0/F4A2、A4A8/F4A8 |
| `mcan_v1` 家族 | A448/F448/F334/F336/A4A8/F4A8，较 K116/K118 多 CCCR.UTSU |

CAN 和 MCAN 是独立寄存器块，A4A8/F4A8 同时有两种。各实例按 SVD `derivedFrom` 展开，
再用 `hc32_ll_can.h` 或 `hc32_ll_mcan.h` 核对功能。FCM 归[时钟组](clock-reset.md)。
