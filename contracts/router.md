## router

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)

## Description 
          
This contract implements router and management of the pools. Due to destributed nature of the Router it can't do many checks, so it's mostly the proxy for the calls

Main idea is that if malformed or corrupted data is sent to the router it would create malformed address of the pool and message sent to it would fail.
So if message reaches the pool it means some criterias are satisfied.


## Data Storage 
| Index |   Type   | Size (b/r) | Cell | Name | Description |
| ---   |  ---     |    ---     | ---  | ---  |    ---      | 
|     1 |    uint1 |  1 /  0 |  1 | router::is_locked | Unused - flag that denotes if the router is locked  |
|     2 |     addr |  267 /  0 |  1 | router::admin_address | Admin address. Only this address can create new pools  |
|     3 |     code |  0 /  1 |  1 | router::poolv3_code | The cell with the code of the pool, that is needed to create a pool contract  |
|     4 |     code |  0 /  1 |  1 | router::accountv3_code | The cell with the code of the account, that is needed to create initial data for pool contract  |
|     5 |     code |  0 /  1 |  1 | router::position_nftv3_code | The cell with the code of the user NFT position, that is needed to create initial data for pool contract  |


### Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 268 | 755 | 

## Interface 
### (cell, cell, cell) getChildContracts ()
 
 
  This function returns if router is locked

  * @return0 1 bit unsigned value 0 - not locked, 1 - locked
-}
int getIsLocked() method_id {
    load_storage();
    return (router::is_locked);
}

{- %GET%
  returns router admin address

  * @return0 router admin address
-}
slice getAdminAddress() method_id {
    load_storage();
    return (router::admin_address);
}

{- %GET%
  returns pool address for two given jetton_wallets beloning to the router 

  * @param jetton_wallet0  Address of the jetton 0 wallet belonging to router
  * @param jetton_wallet1  Address of the jetton 1 wallet belonging to router

  * @return0 pool address
-}

slice getPoolAddress(slice jetton_wallet0, slice jetton_wallet1) method_id {
    load_storage();
    cell state_init = calculate_pool_state_init(jetton_wallet0, jetton_wallet1, router::poolv3_code, router::accountv3_code, router::position_nftv3_code );
    return calculate_address(state_init, 0);
}

{- %GET%
  returns code of the child contracts *deprecated*

  * @return0 code of the pool contract
  * @return1 code of the account contract
  * @return2 code of the nft position contract
  
 