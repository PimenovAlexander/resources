## account

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 

   Account contract. This a contract that stores the funds that user would use for minting, And also proxies the minting message


# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="92">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="64">Cell</th><th>Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>addr</td><td> 267 /  0</td><td>1</td><td>account::user_address</td><td>Address of the user ton wallet of the user that owns this two jetton account  </tr>
<tr><td>2</td><td>addr</td><td> 267 /  0</td><td>1</td><td>account::pool_address</td><td>Address of the pool that created this two jetton account  </tr>
<tr><td>3</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::amount0</td><td>Anount of jetton0 (in pool terms) currently stored in the account  </tr>
<tr><td>4</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::amount1</td><td>Anount of jetton1 (in pool terms) currently stored in the account  </tr>
<tr><td>5</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::enough0</td><td>Anount of jetton0 (in pool terms) that is enought to make a mint operation. When reached for both tokens, mint is triggered  </tr>
<tr><td>6</td><td>coins</td><td> 124 /  0</td><td>11</td><td>account::enough1</td><td>Anount of jetton1 (in pool terms) that is enought to make a mint operation.  </tr>
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
  * @return4 account::enough0         
  * @return5 account::enough1
 
# Messages 

## ACCOUNTV3_ADD_LIQUIDITY 
  * op of type Uint(32) op
  * query_id of type Uint(64) 
  * new_amount0 of type Coins()  
  * new_amount1 of type Coins()  
  * new_enough0 of type Coins()  
  * new_enough1 of type Coins()  
  * liquidity of type Uint(128)
  * tickMin of type Int(24)  
  * tickMax of type Int(24)  

## ACCOUNTV3_RESET_GAS 
