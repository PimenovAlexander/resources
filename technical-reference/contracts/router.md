## router

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 
          
This contract implements the router and does the management of the pools. Due to the distributed nature of the TON Blockchain router, 
it can't do many checks, so it's mostly the proxy for the calls. The router contract so far is used as an owner of all the
wallets holding the funds invested by liquidity providers.

The main idea is that if malformed or corrupted data is sent to the router it would create a malformed address of 
the pool and the message sent to it would fail. So if the message reaches the pool it means some criteria are satisfied.



# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="70">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="58">Cell</th><th width="280">Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>uint1</td><td> 1 /  0</td><td>1</td><td>router::is_locked</td><td>Unused - flag that denotes should denote the router is locked. Currently unused  </tr>
<tr><td>2</td><td>addr</td><td> 267 /  0</td><td>1</td><td>router::admin_address</td><td>Admin address. Only this address can create new pools  </tr>
<tr><td>3</td><td>uint64</td><td> 64 /  0</td><td>1</td><td>router::pool_seqno</td><td>Number of pools created. Used by indexer to ensure that none of pools are skipped  </tr>
<tr><td>4</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::poolv3_code</td><td>The cell with the code of the pool, that is needed to create a pool contract  </tr>
<tr><td>5</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::accountv3_code</td><td>The cell with the code of the account, that is needed to create initial data for pool contract  </tr>
<tr><td>6</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::position_nftv3_code</td><td>The cell with the code of the user NFT position, that is needed to create initial data for pool contract  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 332 | 691 | 

# Interface 
## getIsLocked
 
(int) getIsLocked ()
 
 
  This function returns if router is locked

  * @return0 1 bit unsigned value 0 - not locked, 1 - locked
 
## getAdminAddress
 
(slice) getAdminAddress ()
 
 
  returns router admin address

  * @return0 router admin address
 
## getPoolAddress
 
(slice) getPoolAddress (slice jetton_wallet0, slice jetton_wallet1)
 
 
  returns pool address for two given jetton_wallets belonging to the router 

  * @param jetton_wallet0  Address of the jetton 0 wallet belonging to router
  * @param jetton_wallet1  Address of the jetton 1 wallet belonging to router

  * @return0 pool address
 
## getChildContracts
 
(cell, cell, cell) getChildContracts ()
 
 
  returns code of the child contracts *deprecated*

  * @return0 code of the pool contract
  * @return1 code of the account contract
  * @return2 code of the nft position contract
  
 
# Messages 

## ROUTERV3_CREATE_POOL
Opcode : **0x2e3034ef** 

Operation that deploys and inits new [Pool](pool.md) contract for two given jettons identified by their wallets. New pool would reorder the jettons to match the invariant `slice_hash(jetton0_address) > slice_hash(jetton1_address).`

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| jetton_wallet0 | Address() | Address of the jetton0 wallet. Used to compute pool address | 
| jetton_wallet1 | Address() | Address of the jetton1 wallet. Used to compute pool address | 
| tick_spacing | Int(24)   | Tick spacing to be used in the pool | 
| initial_priceX96 | Uint(160),PriceX96 | Initial price for the pool | 
| nftv3_content | Cell(),Metadata |  | 
| nftv3item_content | Cell(),Metadata |  | 
| jetton0_minter | Address() | Address of the jetton0 minter, used by indexer and frontend | 
| jetton1_minter | Address() | Address of the jetton1 minter, used by indexer and frontend | 
| controller_addr | Address() | Address that is allowed to change the fee. Can always be updated by admin | 

## ROUTERV3_PAY_TO
Opcode : **0xa1daa96d** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  |  | 
| owner | Address() |  | 
| exit_code | Uint(32)  |  | 
| seqno | Uint(64), Indexer |  | 
| hasCoinsInfo | Boolean()  |  | 
| hasIndexerInfo | Boolean()  |  | 
| amount0 | Coins()   |  | 
| jetton0_address | Address() |  | 
| amount1 | Coins()   |  | 
| jetton1_address | Address() |  | 

## ROUTERV3_TRANSFER_NOTIFICATION
Opcode : **0xf189f909** 



| Mnemonic | Type | Description |
| --- | --- | --- |

## ACCOUNTV3_RESET_GAS,POOLV3_RESET_GAS,ROUTERV3_RESET_GAS
Opcode : **0x42a0fb43** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 

## ROUTERV3_CHANGE_ADMIN
Opcode : **0xd4deb03b** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| new_admin | Address() |  | 
