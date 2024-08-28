# Router

### router

* [Description](router.md#description)
* [Data Storage](router.md#data-storage)
* [Interface](router.md#interface)

### Description

This contract implements router and management of the pools. Due to destributed nature of the Router it can't do many checks, so it's mostly the proxy for the calls

Main idea is that if malformed or corrupted data is sent to the router it would create malformed address of the pool and message sent to it would fail. So if message reaches the pool it means some criterias are satisfied.

### Data Storage

<table data-full-width="true"><thead><tr><th>Index</th><th>Type</th><th>Size (b/r)</th><th>Cell</th><th>Name</th><th>Description</th></tr></thead><tbody><tr><td>1</td><td>uint1</td><td>1 / 0</td><td>1</td><td>router::is_locked</td><td>Unused - flag that denotes if the router is locked</td></tr><tr><td>2</td><td>addr</td><td>267 / 0</td><td>1</td><td>router::admin_address</td><td>Admin address. Only this address can create new pools</td></tr><tr><td>3</td><td>code</td><td>0 / 1</td><td>1</td><td>router::poolv3_code</td><td>The cell with the code of the pool, that is needed to create a pool contract</td></tr><tr><td>4</td><td>code</td><td>0 / 1</td><td>1</td><td>router::accountv3_code</td><td>The cell with the code of the account, that is needed to create initial data for pool contract</td></tr><tr><td>5</td><td>code</td><td>0 / 1</td><td>1</td><td>router::position_nftv3_code</td><td>The cell with the code of the user NFT position, that is needed to create initial data for pool contract</td></tr></tbody></table>

#### Cells

| Name | Size | Free |
| ---- | ---- | ---- |
| 1    | 268  | 755  |

### Interface

#### getChildContracts

(cell, cell, cell) getChildContracts ()

This function returns if router is locked

* @return0 1 bit unsigned value 0 - not locked, 1 - locked -} int getIsLocked() method\_id { load\_storage(); return (router::is\_locked); }

{- %GET% returns router admin address

* @return0 router admin address -} slice getAdminAddress() method\_id { load\_storage(); return (router::admin\_address); }

{- %GET% returns pool address for two given jetton\_wallets beloning to the router

* @param jetton\_wallet0 Address of the jetton 0 wallet belonging to router
* @param jetton\_wallet1 Address of the jetton 1 wallet belonging to router
* @return0 pool address -}

slice getPoolAddress(slice jetton\_wallet0, slice jetton\_wallet1) method\_id { load\_storage(); cell state\_init = calculate\_pool\_state\_init(jetton\_wallet0, jetton\_wallet1, router::poolv3\_code, router::accountv3\_code, router::position\_nftv3\_code ); return calculate\_address(state\_init, 0); }

{- %GET% returns code of the child contracts _deprecated_

* @return0 code of the pool contract
* @return1 code of the account contract
* @return2 code of the nft position contract
