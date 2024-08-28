## router

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 
          
This contract implements router and management of the pools. Due to destributed nature of the Router it can't do many checks, so it's mostly the proxy for the calls

Main idea is that if malformed or corrupted data is sent to the router it would create malformed address of the pool and message sent to it would fail.
So if message reaches the pool it means some criterias are satisfied.


# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="92">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="64">Cell</th><th>Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>uint1</td><td> 1 /  0</td><td>1</td><td>router::is_locked</td><td>Unused - flag that denotes if the router is locked  </tr>
<tr><td>2</td><td>addr</td><td> 267 /  0</td><td>1</td><td>router::admin_address</td><td>Admin address. Only this address can create new pools  </tr>
<tr><td>3</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::poolv3_code</td><td>The cell with the code of the pool, that is needed to create a pool contract  </tr>
<tr><td>4</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::accountv3_code</td><td>The cell with the code of the account, that is needed to create initial data for pool contract  </tr>
<tr><td>5</td><td>code</td><td> 0 /  1</td><td>1</td><td>router::position_nftv3_code</td><td>The cell with the code of the user NFT position, that is needed to create initial data for pool contract  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 268 | 755 | 

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
 
 
  returns pool address for two given jetton_wallets beloning to the router 

  * @param jetton_wallet0  Address of the jetton 0 wallet belonging to router
  * @param jetton_wallet1  Address of the jetton 1 wallet belonging to router

  * @return0 pool address
 
## getChildContracts
 
(cell, cell, cell) getChildContracts ()
 
 
  returns code of the child contracts *deprecated*

  * @return0 code of the pool contract
  * @return1 code of the account contract
  * @return2 code of the nft position contract
  
 