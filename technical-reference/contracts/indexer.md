---
hidden: true
---

# 🗂️ Indexer

## 🗂️ Indexer

## TON Events data that is collected

### Pool creaton

#### Opcode

ROUTERV3\_OPERATION\_CREATE\_POOL

| **Value**   | **Type** |
| ----------- | -------- |
| jetton0     | address  |
| jetton1     | address  |
| poolAddress | address  |

#### Opcode

POOLV3\_INIT

| **Value**   | **Type** |
| ----------- | -------- |
| tick        | int      |
| priceSqrt   | uint     |
| tickSpacing | uint     |

### Минт позиции

#### Opcode

POOLV3\_OPERATION\_MINT

| **Value**  | **Type** |
| ---------- | -------- |
| owner      | address  |
| nftID      | uint     |
| nftAddress | address  |
| tickLower  | int      |
| tickUpper  | int      |
| liquidity  | uint     |
| amount0    | uint     |
| amount1    | uint     |

### Берн позиции

#### Opcode

Тут не уверен насчет опкода

| **Value** | **Type** |
| --------- | -------- |
| owner     | address  |
| nftID     | uint     |
| tickLower | int      |
| tickUpper | int      |
| liquidity | uint     |
| amount0   | uint     |
| amount1   | uint     |

### Коллект фи

#### Opcode

POOLV3\_OPERATION\_COLLECT\_PROTOCOL

| **Value** | **Type** |
| --------- | -------- |
| owner     | address  |
| nftID     | uint     |
| tickLower | int      |
| tickUpper | int      |
| liquidity | uint     |
| amount0   | uint     |
| amount1   | uint     |

### Свап

#### Opcode

POOLV3\_OPERATION\_SWAP

| **Value**       | **Type** |
| --------------- | -------- |
| sender          | address  |
| recipient       | address  |
| liquidity       | uint     |
| amount0         | uint     |
| amount1         | uint     |
| price           | uint     |
| tick            | int      |
| totalFeeGrowth0 | int      |
| totalFeeGrowth1 | int      |
