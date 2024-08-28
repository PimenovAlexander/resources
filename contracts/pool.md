## pool

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)

### Description 

This contract implements V3 like functionality of the pool with concentrated liquidity. 

Due to new storage organisation and availability of the **dict** data type we don't need the tickBitmap data structures. However gas consumption while using the dict is also quite siginificant

### Data Storage 
| Index |   Type   | Size (b/r) | Cell | Name | Description |
| ---   |  ---     |    ---     | ---  | ---  |    ---      | 
|     1 |     addr |  267 /  0 |  1 | poolv3::router_address | Address of the router contract that created this pool  |
|     2 |   uint16 |  16 /  0 |  1 | poolv3::lp_fee_base | Liquidity provider fee. base in FEE_DENOMINATOR parts  |
|     3 |   uint16 |  16 /  0 |  1 | poolv3::protocol_fee | Protocol fee in FEE_DENOMINATOR (16 bit)  |
|     4 |   uint16 |  16 /  0 |  1 | poolv3::lp_fee_current | current value of the pool fee, in case of dynamic adjustment  |
|     5 |     addr |  267 /  0 |  1 | poolv3::jetton0_wallet | Address of the 0 token in the pool. This is an address of the jetton0 wallet attached to router  |
|     6 |     addr |  267 /  0 |  1 | poolv3::jetton1_wallet | Address of the 1 token in the pool  This is an address of the jetton0 wallet attached to router  |
|     7 |    int24 |  24 /  0 |  1 | poolv3::tick_spacing | Spacing of the ticks, 24 bits of signed int would be used  |
|     8 |  uint256 |  256 /  0 | 11 | poolv3::feeGrowthGlobal0X128 | This variable stores current collected fee per the unit of liquidity in jetton0  |
|     9 |  uint256 |  256 /  0 | 11 | poolv3::feeGrowthGlobal1X128 | This variable stores current collected fee per the unit of liquidity in jetton0  |
|    10 |  uint128 |  128 /  0 | 11 | poolv3::collectedProtocolFee0 | Collected protocol fee of the jetton0  |
|    11 |  uint128 |  128 /  0 | 11 | poolv3::collectedProtocolFee1 | Collected protocol fee of the jetton1  |
|    12 |    coins |  124 /  0 | 11 | poolv3::reserve0 | These are additional separate protection system - it calculates the reserves of the pool <br/>   In case main math has a bug it protects the funds of other pools. Reserve of the jetton0  |
|    13 |    coins |  124 /  0 | 11 | poolv3::reserve1 | Reserve of the jetton1  |
|    14 |    uint1 |  1 /  0 |  2 | poolv3::pool_active | Pool acitve flag 0 is inactive, 1 is active  |
|    15 |    int24 |  24 /  0 |  2 | poolv3::tick | Current tick, signed, 24 bits would be used. Pool maintains it in correspondance to poolv3::price_sqrt  |
|    16 |  uint160 |  160 /  0 |  2 | poolv3::price_sqrt | Current square root of the price in Q64.96 format using 160 bits  |
|    17 |  uint128 |  128 /  0 |  2 | poolv3::liquidity | Current active concentrated liquidity in 128 bits  |
|    18 |   uint24 |  24 /  0 |  2 | poolv3::occupied_ticks | number of occupied ticks in storage  |
|    19 |   uint64 |  64 /  0 |  2 | poolv3::nftv3item_counter | Pool is also an NFT collection. So this is the counter of currently minted positions  |
|    20 |   uint64 |  64 /  0 |  2 | poolv3::nftv3items_active | Pool is also an NFT collection. Number of unburnt items  |
|    21 |     addr |  267 /  0 |  2 | poolv3::admin_address | Admin address. Can init pool  |
|    22 |     addr |  267 /  0 |  2 | poolv3::controller_address | Controller address. Can change fee and lock and unlock pool  |
|    23 |     addr |  267 /  0 | 21 | poolv3::jetton0_minter | Address of the 0 token minter. Beta only.  |
|    24 |     addr |  267 /  0 | 21 | poolv3::jetton1_minter | Address of the 1 token minter. Beta only.  |
|    25 |     dict |  0 /  1 |  3 | poolv3::ticks_dictionary | Storage of the ticks   |
|    26 |     code |  0 /  1 |  4 | poolv3::accountv3_code | Pool knows how to create user accounts to store fund pairs for a particular user  |
|    27 |     code |  0 /  1 |  4 | poolv3::position_nftv3_code | Pool knows how to create user position. So it stores it's code  |
|    28 |     cell |  0 /  1 |  4 | poolv3::nftv3_content | packed metadata that would be given to nft that corresponds to nft colletion  |
|    29 |     cell |  0 /  1 |  4 | poolv3::nftv3item_content | packed metadata that would be given to nft that corresponds to the position  |


### Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 873 | 150 | 
| 2  | 999 | 24 | 
| 3  | 0 | 1023 | 
| 4  | 0 | 1023 | 
| 11  | 1016 | 7 | 
| 21  | 534 | 489 | 

## Interface 
### (slice, slice, 
 slice, slice, slice, slice,
 int, int,
 int, int, int,
 int, int, int,
 int, int, int, int, 
 int,
 int, int,
 int, int 
 ) getPoolStateAndConfiguration ()
 
  
    Returns is pool is active

    @return0 int containing the poolv3::pool_active  
-}
int getIsActive() method_id {
    load_storage();
    return (poolv3::pool_active);
}

{- %GET% 
    Returns pool state and configuration

    @return0 int containing the poolv3::pool_active  
 
### (tuple, tuple) getTickInfosFrom (int key, int amount, int dir)
 
  
   Returns amount ticks starting form key (non-inclusive)
   0 forward 
   1 backward
 