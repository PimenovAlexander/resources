## account

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 

   Account contract. This a contract that stores the funds that the user would use for minting. 
   The account also proxies the minting message and collects proof that both jettons were actually funded correctly

# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="70">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="58">Cell</th><th width="280">Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>addr</td><td> 267 /  0</td><td>1</td><td>account::user_address</td><td>Address of the user ton wallet of the user that owns this two jetton account  </tr>
<tr><td>2</td><td>addr</td><td> 267 /  0</td><td>1</td><td>account::pool_address</td><td>Address of the pool that created this two jetton account  </tr>
<tr><td>3</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::amount0</td><td>Amount of jetton0 (in pool terms) currently stored in the account  </tr>
<tr><td>4</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::amount1</td><td>Amount of jetton1 (in pool terms) currently stored in the account  </tr>
<tr><td>5</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::enough0</td><td>Amount of jetton0 (in pool terms) that is enough to make a mint operation. When reached for both tokens, mint is triggered  </tr>
<tr><td>6</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::enough1</td><td>Amount of jetton1 (in pool terms) that is enough to make a mint operation.  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 534 | 489 | 
| 11  | 496 | 527 | 

# Interface 
## get_account_data
 
(slice, slice, int, int, int, int) get_account_data ()
 
 

  This function provides current state of the user account

  * @return0 account::user_address   Address of the owner of the account
  * @return1 account::pool_address   Address of the pool that this account is attached to
  * @return2 account::amount0        Amount of jetton0 that was deposited for mint
  * @return3 account::amount1        Amount of jetton1 that was deposited for mint
  * @return4 account::enough0        Amount of jetton0 that is enough to send message to the pool and actually do the mint  
  * @return5 account::enough1        Amount of jetton1 that is enough to send message to the pool and actually do the mint
 
# Messages 

## ACCOUNTV3_ADD_LIQUIDITY
Opcode : **0x3ebe5431** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  |  | 
| new_amount0 | Coins()   |  | 
| new_amount1 | Coins()   |  | 
| new_enough0 | Coins()   |  | 
| new_enough1 | Coins()   |  | 
| liquidity | Uint(128) |  | 
| tickMin | Int(24)   |  | 
| tickMax | Int(24)   |  | 

## ACCOUNTV3_RESET_GAS,POOLV3_RESET_GAS,ROUTERV3_RESET_GAS
Opcode : **0x42a0fb43** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  |  | 
