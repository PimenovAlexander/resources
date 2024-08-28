## account
### Description 

   Account contract. This a contract that stores the funds that user would use for minting, And also proxies the minting message


### Data Storage 
Location [accountv3](../contracts/accountv3)
| Index |   Type   | Size (b/r) | Cell | Name | Description |
| ---   |  ---     |    ---     | ---  | ---  |    ---      | 
|     1 |     addr |  267 /  0 |  1 | account::user_address | Address of the user ton wallet of the user that owns this two jetton account  |
|     2 |     addr |  267 /  0 |  1 | account::pool_address | Address of the pool that created this two jetton account  |
|     3 |    coins |  124 /  0 | 11 | account::amount0 | Anount of jetton0 (in pool terms) currently stored in the account  |
|     4 |    coins |  124 /  0 | 11 | account::amount1 | Anount of jetton1 (in pool terms) currently stored in the account  |
|     5 |    coins |  124 /  0 | 11 | account::enough0 | Anount of jetton0 (in pool terms) that is enought to make a mint operation. When reached for both tokens, mint is triggered  |
|     6 |    coins |  124 /  0 | 11 | account::enough1 | Anount of jetton1 (in pool terms) that is enought to make a mint operation.  |
### Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 534 | 489 | 
| 11  | 496 | 527 | 
