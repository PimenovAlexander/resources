## pool

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 

This contract implements V3 like functionality of the pool with concentrated liquidity. 

Due to the new storage organization and availability of the **dict** data type we don't need the tickBitmap data structures. However, gas consumption while using the dict is also quite significant

# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="70">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="58">Cell</th><th width="280">Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::router_address</td><td>Address of the router contract that created this pool  </tr>
<tr><td>2</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::lp_fee_base</td><td>Liquidity provider fee. base in FEE_DENOMINATOR parts  </tr>
<tr><td>3</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::protocol_fee</td><td>Protocol fee in FEE_DENOMINATOR (16 bit)  </tr>
<tr><td>4</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::lp_fee_current</td><td>Current value of the pool fee, in case of dynamic adjustment  </tr>
<tr><td>5</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::jetton0_wallet</td><td>Address of the 0 token in the pool. This is an address of the jetton0 wallet attached to router  </tr>
<tr><td>6</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::jetton1_wallet</td><td>Address of the 1 token in the pool  This is an address of the jetton0 wallet attached to router  </tr>
<tr><td>7</td><td>int24</td><td> 24 /  0</td><td>1</td><td>poolv3::tick_spacing</td><td>Spacing of the ticks, 24 bits of signed int would be used  </tr>
<tr><td>8</td><td>uint64</td><td> 64 /  0</td><td>1</td><td>poolv3::seqno</td><td>Used by indexer to ensure that none of pool interactions are skipped  </tr>
<tr><td>9</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>poolv3::feeGrowthGlobal0X128</td><td>This variable stores current collected fee per the unit of liquidity in jetton0  </tr>
<tr><td>10</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>poolv3::feeGrowthGlobal1X128</td><td>This variable stores current collected fee per the unit of liquidity in jetton0  </tr>
<tr><td>11</td><td>uint128</td><td> 128 /  0</td><td>11</td><td>poolv3::collectedProtocolFee0</td><td>Collected protocol fee of the jetton0  </tr>
<tr><td>12</td><td>uint128</td><td> 128 /  0</td><td>11</td><td>poolv3::collectedProtocolFee1</td><td>Collected protocol fee of the jetton1  </tr>
<tr><td>13</td><td>coins</td><td> 124 /  0</td><td>11</td><td>poolv3::reserve0</td><td>These are additional separate protection system - it calculates the reserves of the pool <br/>   In case main math has a bug it protects the funds of other pools. Reserve of the jetton0  </tr>
<tr><td>14</td><td>coins</td><td> 124 /  0</td><td>11</td><td>poolv3::reserve1</td><td>Reserve of the jetton1  </tr>
<tr><td>15</td><td>uint1</td><td> 1 /  0</td><td>2</td><td>poolv3::pool_active</td><td>Pool acitve flag 0 is inactive, 1 is active  </tr>
<tr><td>16</td><td>int24</td><td> 24 /  0</td><td>2</td><td>poolv3::tick</td><td>Current tick, signed, 24 bits would be used. Pool maintains it in correspondence to poolv3::price_sqrt  </tr>
<tr><td>17</td><td>uint160</td><td> 160 /  0</td><td>2</td><td>poolv3::price_sqrt</td><td>Current square root of the price in Q64.96 format using 160 bits  </tr>
<tr><td>18</td><td>uint128</td><td> 128 /  0</td><td>2</td><td>poolv3::liquidity</td><td>Current active concentrated liquidity in 128 bits  </tr>
<tr><td>19</td><td>uint24</td><td> 24 /  0</td><td>2</td><td>poolv3::occupied_ticks</td><td>number of occupied ticks in storage  </tr>
<tr><td>20</td><td>uint64</td><td> 64 /  0</td><td>2</td><td>poolv3::nftv3item_counter</td><td>Pool is also an NFT collection. So this is the counter of currently minted positions  </tr>
<tr><td>21</td><td>uint64</td><td> 64 /  0</td><td>2</td><td>poolv3::nftv3items_active</td><td>Pool is also an NFT collection. Number of unburnt items  </tr>
<tr><td>22</td><td>addr</td><td> 267 /  0</td><td>2</td><td>poolv3::admin_address</td><td>Admin address. Can init pool  </tr>
<tr><td>23</td><td>addr</td><td> 267 /  0</td><td>2</td><td>poolv3::controller_address</td><td>Controller address. Can change fee and lock and unlock pool  </tr>
<tr><td>24</td><td>addr</td><td> 267 /  0</td><td>21</td><td>poolv3::jetton0_minter</td><td>Address of the 0 token minter. Beta only.  </tr>
<tr><td>25</td><td>addr</td><td> 267 /  0</td><td>21</td><td>poolv3::jetton1_minter</td><td>Address of the 1 token minter. Beta only.  </tr>
<tr><td>26</td><td>dict</td><td> 0 /  1</td><td>3</td><td>poolv3::ticks_dictionary</td><td>Storage of the ticks   </tr>
<tr><td>27</td><td>code</td><td> 0 /  1</td><td>4</td><td>poolv3::accountv3_code</td><td>Pool knows how to create user accounts to store fund pairs for a particular user  </tr>
<tr><td>28</td><td>code</td><td> 0 /  1</td><td>4</td><td>poolv3::position_nftv3_code</td><td>Pool knows how to create user position. So it stores it's code  </tr>
<tr><td>29</td><td>cell</td><td> 0 /  1</td><td>4</td><td>poolv3::nftv3_content</td><td>packed metadata that would be given to nft that corresponds to nft collection  </tr>
<tr><td>30</td><td>cell</td><td> 0 /  1</td><td>4</td><td>poolv3::nftv3item_content</td><td>packed metadata that would be given to nft that corresponds to the position  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 937 | 86 | 
| 2  | 999 | 24 | 
| 3  | 0 | 1023 | 
| 4  | 0 | 1023 | 
| 11  | 1016 | 7 | 
| 21  | 534 | 489 | 

# Interface 
## getIsActive
 
(int) getIsActive ()
 
  
  Returns is pool is active

  * @return0 int containing the poolv3::pool_active  
 
## getPoolStateAndConfiguration
 
(slice, slice, slice, slice, slice, slice, slice, int, int, int, int, int, int, int, int, int, int, int, int,  int, int, int, int, int, int  ) getPoolStateAndConfiguration ()
 
  
  Returns pool state and configuration

  * @return0 Router address  
  * @return1 Admin address  
  * @return2 Jetton 0 Wallet address. The wallet is owned by the Router
  * @return3 Jetton 1 Wallet address. The wallet is owned by the Router
  * @return4 Jetton 0 Minter address. 
  * @return5 Jetton 1 Minter address. 
  * @return6 Flag that denotes if the pool is active
  * @return7 Pool tick spacing
  
  * @return8  Fee that is used as a base. Stored in x 1/10000 
  * @return9  Fee that protocol takes. Stored in x 1/10000 
  * @return10 Fee that is currently active. Stored in x 1/10000 
  
  * @return11 Current tick 
  * @return12 Current price
  * @return13 Current liquidity

  * @return14  poolv3::feeGrowthGlobal0X128
  * @return15  poolv3::feeGrowthGlobal1X128
  * @return16 Amount of jetton0 fee collected for protocol owners
  * @return17 Amount of jetton0 fee collected for protocol owners

  * @return18 Number of total minted NFT positions

  * @return19 Reserves of the jetton0
  * @return20 Reserves of the jetton1

  * @return21 Number of active NFT positions
  * @return22 Number of currently occupied ticks

  * @return22 Number of operations with pool since the deploy
 
## getChildContracts
 
(cell, cell, cell, cell) getChildContracts ()
 
 
  * @return1 code of the account contract
  * @return2 code of the nft position contract  
  * @return3 metadata for NFT Collection
  * @return4 metadata for NFT Item
 
## getAllTickInfos
 
(cell) getAllTickInfos ()
 
 
  returns the cell with all the ticks.

  * @return1 cell that contains the dict with all the ticks.
 
## getTickInfosFrom
 
(tuple) getTickInfosFrom (int key, int amount, int dir, int full)
 
  
   Returns ticks starting form key (non-inclusive)
   0 forward 
   1 backward

  * @param key
  * @param amount
  * @param dir
  * @param full   

  * @return0 a tuple with keys
  * @return1 a tuple with corresponding 
    * liquidityTotal 
    * liquidityDelta 
    * outerFeeGrowth0Token
    * outerFeeGrowth1Token
 
## getCollectedFees
 
(int, int) getCollectedFees (int tickLower, int tickUpper, int posLiquidityDelta, int posFeeGrowthInside0X128, int posFeeGrowthInside1X128)
 
  
  Predicts how much fees a position can collect
  
  * @param tickLower
  * @param tickUpper
  * @param posLiquidityDelta
  * @param posFeeGrowthInside0X128
  * @param posFeeGrowthInside1X128

  * @return0 amount of jetton0 that is collected
  * @return1 amount of jetton1 that is collected

 
## getUserAccountAddress
 
(slice) getUserAccountAddress (slice user_address)
 
  
   computes user account address for a given user

   @return0 account address   
 
## getMintEstimate
 
(int, int, int) getMintEstimate (int tickLower, int tickUpper, int liquidity)
 
  
  Computes estimates for the mint

  * @param  tickLower
  * @param  tickUpper
  * @param  liquidity
  
  * @return0 amount of jetton0 needed to mint the position 
  * @return1 amount of jetton1 needed to mint the position
  * @return2 error code that shows if the basic checks for the mint would succeed 
   
 
## getSwapEstimate
 
(int, int) getSwapEstimate (int zeroForOne, int amount, int sqrtPriceLimitX96)
 
  
  **Deprecated**
  Computes estimates fot the swap

  * @param  zeroForOne
  * @param  amount
  * @param  sqrtPriceLimitX96
  
  * @return0 amount of jetton0 that would be put to/get from the pool
  * @return1 amount of jetton1 that would be put to/get from the pool
 
## getSwapEstimateGas
 
(int, int) getSwapEstimateGas (int zeroForOne, int amount, int sqrtPriceLimitX96,  int minOutAmount, int gasLimit)
 
  
  Computes estimates for the swap

  * @param  zeroForOne
  * @param  amount
  * @param  sqrtPriceLimitX96
  * @param  minAmountOut
  * @param  gasLimit   amount of gas (in gas units). If gasLimit is 0 - default value is used - that equals to contract gas limit from the config 
  
  * @return0 amount of jetton0 that would be put to/get from the pool
  * @return1 amount of jetton1 that would be put to/get from the pool
 
## get_collection_data
 
(int, cell, slice) get_collection_data ()
 
  
  In accordance with TEP-62

  * @return0 
  * @return1 
  * @return2 
 
## get_nft_address_by_index
 
(slice) get_nft_address_by_index (int index)
 
  
  In accordance with TEP-62

  * @return0   
 
## get_nft_content
 
(cell) get_nft_content (int index, cell nftv3item_content)
 
  
   In accordance with TEP-62 we will form the output NFT onchain metadata
   
   If NFTItemContent "description" field that is passed to the pool by the pool administrator (poolv3::nftv3item_content) begins with marker "%N%", data from 
   the NFT is added to the beginning of the "description" field. If there is no data attached to the dictionary by the NFT, NFTItemContent description is unchanged

  * @return0   
 
# Messages 

## POOLV3_INIT
Opcode : **0x441c39ed** 

The first mandatory operation that fills crucial parameters of the pool

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| has_admin | UInt(1)  | Flag that shows if this message have a new admin address | 
| admin_addr | Address() | New address of the admin. If has_admin is false could be 00b | 
| has_controller | UInt(1)  | Flag that shows if this message have a new controller address | 
| controller_addr | Address() | Address that is allowed to change the fee. Can always be updated by admin. If has_controller is false could be 00b | 
| set_spacing | UInt(1)  | Flag that shows if tick_spacing should be set to the pool or ignored | 
| tick_spacing | Int(24)    | Tick spacing to be used in the pool | 
| set_price | UInt(1)  | Flag that shows if initial_priceX96 should be set to the pool or ignored | 
| initial_priceX96 | Uint(160),PriceX96 | Initial price for the pool | 
| set_active | UInt(1)  | Flag that shows if pool_active should be set to the pool or ignored | 
| pool_active | UInt(1)  | Flag is we should start the pool as unlocked | 
| nftv3_content | Cell(),Metadata |  | 
| nftv3item_content | Cell(),Metadata |  | 
| has_minters | UInt(1)  | Flag that stores if this message has minters | 
| jetton0_minter | Address() | Address of the jetton0 minter, used by indexer and frontend | 
| jetton1_minter | Address() | Address of the jetton1 minter, used by indexer and frontend | 

## POOLV3_SET_FEE
Opcode : **0x6bdcbeb8** 

This operation sets the fee values for the pool. This is allowed to do by the operator and the admin

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| protocol_fee | Uint(16)  | Liquidity provider fee. base in FEE_DENOMINATOR parts | 
| lp_fee_base | Uint(16)  | Protocol fee in FEE_DENOMINATOR  | 
| lp_fee_current | Uint(16)  | Current value of the pool fee, in case of dynamic adjustment | 

## POOLV3_LOCK
Opcode : **0x5e74697** 

This operation locks the pool. This is allowed to do by the operator and the admin

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 

## POOLV3_UNLOCK
Opcode : **0x3205adbd** 

This operation unlocks the pool. This is allowed to do by the operator and the admin

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 

## POOLV3_FUND_ACCOUNT
Opcode : **0x4468de77** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)   | queryid as of the TON documentation | 
| owner_addr | Address()  | Address that would receive the minted NFT, excesses and refunds | 
| amount0 | Coins()    |  | 
| amount1 | Coins()    |  | 
| enough0 | Coins()    |  | 
| enough1 | Coins()    |  | 
| liquidity | Uint(128)  | Amount of liquidity to mint | 
| tickLower | Int(24)    | lower bound of the range in which to mint | 
| tickUpper | Int(24)    | upper bound of the range in which to mint | 

## POOLV3_MINT
Opcode : **0x81702ef8** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| amount0Funded | Coins()   | Amount of jetton 0 received by router for the mint | 
| amount1Funded | Coins()   | Amount of jetton 1 recived by router for the mint | 
| recipient | Address() | Address that would receive the minted NFT, excesses and refunds | 
| liquidity | Uint(128) | Amount of liquidity to mint | 
| tickLower | Int(24)   | lower bound of the range in which to mint | 
| tickUpper | Int(24)   | upper bound of the range in which to mint | 

## POOLV3_START_BURN
Opcode : **0x530b5f2c** 

Burn whole or part of nft. Can be called by anyone, but if not called be the owner - would fail later. This operation would compute the amount of the fees that the position is eligible to get and then forwards a message to the [Position NFT](position_nft.md) contract

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| burned_index | Uint(64)  | Index if the NFT to burn | 
| liquidity2Burn | Uint(128) | Amount of the liquidity to burn, 0 is a valid amount, in this case only collected fees would be returned | 
| tickLower | Int(24)   | Lower tick of the NFT. Should match the real one | 
| tickUpper | Int(24)   | Upper tick of the NFT. Should match the real one | 

## POOLV3_BURN
Opcode : **0xd73ac09d** 

Burn whole or part of nft. Is sent by [Position NFT](position_nft.md) itself, would only be accepted from the correct NFT itself

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| recipient | Address() | NFT owner to receive funds | 
| burned_index | Uint(64)  | Index if the NFT to burn. Should match the sender address | 
| liquidity | Uint(128) | NFT liquidity amount prior to burn | 
| tickLower | Int(24)   | Lower tick of the NFT. Sanitized by NFTPosition  contract | 
| tickUpper | Int(24)   | Upper tick of the NFT. Sanitized by NFTPosition  contract | 
| liquidity2Burn | Uint(128) | Amount of the liquidity to burn, 0 is a valid amount, in this case only collected fees would be returned | 
| feeGrowthInside0LastX128 | Uint(256) |  | 
| feeGrowthInside1LastX128 | Uint(256) |  | 
| feeGrowthInside0CurrentX128 | Uint(256),Indexer |  | 
| feeGrowthInside1CurrentX128 | Uint(256),Indexer |  | 

## POOLV3_SWAP
Opcode : **0xa7fb58f8** 

Computes the swap math, and issues a command to the router to send funds. Only would be accepted from the router

| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  | queryid as of the TON documentation | 
| source_wallet | Address() | jetton wallet attached to the router | 
| amount | Coins()   | Input amount of the jettons to be swapped | 
| sqrtPriceLimitX96 | Uint(160),PriceX96 | Limit marginal price. Swap won't go beyond it. | 
| minOutAmount | Coins()   | Minimum amount of the output jettons to get back. If not reached, your input would be returned to you | 
| from_real_user | Address()  |  | 
