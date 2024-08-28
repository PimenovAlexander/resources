## position_nft
### Description 

This is a modifed NFT contract to store user position

### Data Storage 
Location [positionnftv3](../contracts/positionnftv3)
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
