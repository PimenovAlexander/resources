# Router

### router

* [Description](router.md#description)
* [Data Storage](router.md#data-storage)
* [Interface](router.md#interface)
* [Messages](router.md#messages)

## Description

This contract implements the router and does the management of the pools. Due to the distributed nature of the TON Blockchain router, it can't do many checks, so it's mostly the proxy for the calls. The router contract so far is used as an owner of all the wallets holding the funds invested by liquidity providers.

The main idea is that if malformed or corrupted data is sent to the router it would create a malformed address of the pool and the message sent to it would fail. So if the message reaches the pool it means some criteria are satisfied.

## Data Storage

<table data-full-width="true"><thead><tr><th width="70">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="58">Cell</th><th width="280">Name</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td>uint1</td><td>1 / 0</td><td>1</td><td>router::is_locked</td><td>Unused - flag that denotes if the router is locked</td></tr><tr><td>2</td><td>addr</td><td>267 / 0</td><td>1</td><td>router::admin_address</td><td>Admin address. Only this address can create new pools</td></tr><tr><td>3</td><td>uint64</td><td>64 / 0</td><td>1</td><td>router::pool_seqno</td><td>Number of pools created. Used by indexer to ensure that none of pools are skipped</td></tr><tr><td>4</td><td>code</td><td>0 / 1</td><td>1</td><td>router::poolv3_code</td><td>The cell with the code of the pool, that is needed to create a pool contract</td></tr><tr><td>5</td><td>code</td><td>0 / 1</td><td>1</td><td>router::accountv3_code</td><td>The cell with the code of the account, that is needed to create initial data for pool contract</td></tr><tr><td>6</td><td>code</td><td>0 / 1</td><td>1</td><td>router::position_nftv3_code</td><td>The cell with the code of the user NFT position, that is needed to create initial data for pool contract</td></tr></tbody></table>

## Cells

| Name | Size | Free |
| ---- | ---- | ---- |
| 1    | 332  | 691  |

## Interface

### getIsLocked

(int) getIsLocked ()

This function returns if router is locked

* @return0 1 bit unsigned value 0 - not locked, 1 - locked

### getAdminAddress

(slice) getAdminAddress ()

returns router admin address

* @return0 router admin address

### getPoolAddress

(slice) getPoolAddress (slice jetton\_wallet0, slice jetton\_wallet1)

returns pool address for two given jetton\_wallets beloning to the router

* @param jetton\_wallet0 Address of the jetton 0 wallet belonging to router
* @param jetton\_wallet1 Address of the jetton 1 wallet belonging to router
* @return0 pool address

### getChildContracts

(cell, cell, cell) getChildContracts ()

returns code of the child contracts _deprecated_

* @return0 code of the pool contract
* @return1 code of the account contract
* @return2 code of the nft position contract

## Messages

### ROUTERV3\_CREATE\_POOL

Opcode : **0x2e3034ef**

| Mnemonic           | Type               | Description               |
| ------------------ | ------------------ | ------------------------- |
| op                 | Uint(32) op        |                           |
| query\_id          | Uint(64)           |                           |
| jetton\_wallet0    | Address()          |                           |
| jetton\_wallet1    | Address()          |                           |
| tick\_spacing      | Int(24)            |                           |
| initial\_priceX96  | Uint(160),PriceX96 | Inital price for the pool |
| nftv3\_content     | Cell(),Metadata    |                           |
| nftv3item\_content | Cell(),Metadata    |                           |
| jetton0\_minter    | Address()          |                           |
| jetton1\_minter    | Address()          |                           |
| controller\_addr   | Address()          |                           |

### ROUTERV3\_PAY\_TO

Opcode : **0xf93bb43f**

| Mnemonic   | Type        | Description |
| ---------- | ----------- | ----------- |
| op         | Uint(32) op |             |
| query\_id  | Uint(64)    |             |
| owner      | Address()   |             |
| exit\_code | Uint(32)    |             |

### ROUTERV3\_TRANSFER\_NOTIFICATION

Opcode : **0xf189f909**
