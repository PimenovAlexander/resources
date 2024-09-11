# 🗂️ Indexer

# TON Events data that is collected

## Pool creaton

### Opcode

ROUTERV3_OPERATION_CREATE_POOL

| **Value** | **Type** |
| --- | --- |
| jetton0 | address |
| jetton1 | address |
| poolAddress | address |

### Opcode

POOLV3_INIT

| **Value** | **Type** |
| --- | --- |
| tick | int |
| priceSqrt | uint |
| tickSpacing | uint |

## Минт позиции

### Opcode

POOLV3_OPERATION_MINT

| **Value** | **Type** |
| --- | --- |
| owner | address |
| nftID | uint |
| nftAddress | address |
| tickLower | int |
| tickUpper | int |
| liquidity | uint |
| amount0 | uint |
| amount1 | uint |

## Берн позиции

### Opcode

Тут не уверен насчет опкода

| **Value** | **Type** |
| --- | --- |
| owner | address |
| nftID | uint |
| tickLower | int |
| tickUpper | int |
| liquidity | uint |
| amount0 | uint |
| amount1 | uint |

## Коллект фи

### Opcode

POOLV3_OPERATION_COLLECT_PROTOCOL

| **Value** | **Type** |
| --- | --- |
| owner | address |
| nftID | uint |
| tickLower | int |
| tickUpper | int |
| liquidity | uint |
| amount0 | uint |
| amount1 | uint |

## Свап

### Opcode

POOLV3_OPERATION_SWAP


| **Value** | **Type** |
| --- | --- |
| sender | address |
| recipient | address |
| liquidity | uint |
| amount0 | uint |
| amount1 | uint |
| price | uint |
| tick | int |
| totalFeeGrowth0 | int |
| totalFeeGrowth1 | int |