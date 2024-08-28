## position_nft

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)

## Description 

This is a modifed NFT contract to store user position

## Data Storage 
| Index |   Type   | Size (b/r) | Cell | Name | Description |
| ---   |  ---     |    ---     | ---  | ---  |    ---      | 
|     1 |   uint64 |  64 /  0 |  1 | positionv3::index | The position number. Also the nft index  |
|     2 |     addr |  267 /  0 |  1 | positionv3::pool_address | Address of the pool that created this NFT  |
|     3 |     addr |  267 /  0 |  1 | positionv3::user_address | Address of the user ton wallet that currently owns the position  |
|     4 |     cell |  0 /  1 |  1 | positionv3::content | NFT metadata that contains image url, name and description packed in standart format  |
|     5 |  uint128 |  128 /  0 |  1 | positionv3::liquidity | Position liquidity  |
|     6 |    int24 |  24 /  0 |  1 | positionv3::tickLower | Positioni lower tick number  |
|     7 |    int24 |  24 /  0 |  1 | positionv3::tickUpper | Positioni upper tick number  |
|     8 |  uint256 |  256 /  0 | 11 | positionv3::feeGrowthInside0LastX128 | Fees collected before the position was opened or updated for jetton0 (in pool terms)  |
|     9 |  uint256 |  256 /  0 | 11 | positionv3::feeGrowthInside1LastX128 | Fees collected before the position was opened or updated for jetton1 (in pool terms)  |


### Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 774 | 249 | 
| 11  | 512 | 511 | 

## Interface 
### (slice) getPoolAddress ()
 
 
  This function returns pool address that created this Position NFT

  * @return0 address in question 
 
### (slice) getUserAddress ()
 
 
  This function returns user address that owned created this Position NFT

  * @return0 address in question 
 
### (int, int, int, int, int) getPositionInfo ()
 
 
  This function returns data stored in Position NFT and is related to the position

  * @return0 liquidity that this position owns
  * @return1 lower tick of the position
  * @return2 upper tick of the position
  * @return3 fee growth of jetton0 in the given range at moment of the creation or latest collect of the NFT position
  * @return4 fee growth of jetton1 in the given range at moment of the creation or latest collect of the NFT position
 
### (int, int, slice, slice, cell) get_nft_data ()
 
 
  This function returns data of this Position NFT that is related to NFT as TEP-62
 