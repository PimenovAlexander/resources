## pool

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)

# Description 

This contract implements V3 like functionality of the pool with concentrated liquidity. 

Due to new storage organisation and availability of the **dict** data type we don't need the tickBitmap data structures. However gas consumption while using the dict is also quite siginificant

# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="92">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="64">Cell</th><th>Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::router_address</td><td>Address of the router contract that created this pool  </tr>
<tr><td>2</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::lp_fee_base</td><td>Liquidity provider fee. base in FEE_DENOMINATOR parts  </tr>
<tr><td>3</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::protocol_fee</td><td>Protocol fee in FEE_DENOMINATOR (16 bit)  </tr>
<tr><td>4</td><td>uint16</td><td> 16 /  0</td><td>1</td><td>poolv3::lp_fee_current</td><td>current value of the pool fee, in case of dynamic adjustment  </tr>
<tr><td>5</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::jetton0_wallet</td><td>Address of the 0 token in the pool. This is an address of the jetton0 wallet attached to router  </tr>
<tr><td>6</td><td>addr</td><td> 267 /  0</td><td>1</td><td>poolv3::jetton1_wallet</td><td>Address of the 1 token in the pool  This is an address of the jetton0 wallet attached to router  </tr>
<tr><td>7</td><td>int24</td><td> 24 /  0</td><td>1</td><td>poolv3::tick_spacing</td><td>Spacing of the ticks, 24 bits of signed int would be used  </tr>
<tr><td>8</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>poolv3::feeGrowthGlobal0X128</td><td>This variable stores current collected fee per the unit of liquidity in jetton0  </tr>
<tr><td>9</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>poolv3::feeGrowthGlobal1X128</td><td>This variable stores current collected fee per the unit of liquidity in jetton0  </tr>
<tr><td>10</td><td>uint128</td><td> 128 /  0</td><td>11</td><td>poolv3::collectedProtocolFee0</td><td>Collected protocol fee of the jetton0  </tr>
<tr><td>11</td><td>uint128</td><td> 128 /  0</td><td>11</td><td>poolv3::collectedProtocolFee1</td><td>Collected protocol fee of the jetton1  </tr>
<tr><td>12</td><td>coins</td><td> 124 /  0</td><td>11</td><td>poolv3::reserve0</td><td>These are additional separate protection system - it calculates the reserves of the pool <br/>   In case main math has a bug it protects the funds of other pools. Reserve of the jetton0  </tr>
<tr><td>13</td><td>coins</td><td> 124 /  0</td><td>11</td><td>poolv3::reserve1</td><td>Reserve of the jetton1  </tr>
<tr><td>14</td><td>uint1</td><td> 1 /  0</td><td>2</td><td>poolv3::pool_active</td><td>Pool acitve flag 0 is inactive, 1 is active  </tr>
<tr><td>15</td><td>int24</td><td> 24 /  0</td><td>2</td><td>poolv3::tick</td><td>Current tick, signed, 24 bits would be used. Pool maintains it in correspondance to poolv3::price_sqrt  </tr>
<tr><td>16</td><td>uint160</td><td> 160 /  0</td><td>2</td><td>poolv3::price_sqrt</td><td>Current square root of the price in Q64.96 format using 160 bits  </tr>
<tr><td>17</td><td>uint128</td><td> 128 /  0</td><td>2</td><td>poolv3::liquidity</td><td>Current active concentrated liquidity in 128 bits  </tr>
<tr><td>18</td><td>uint24</td><td> 24 /  0</td><td>2</td><td>poolv3::occupied_ticks</td><td>number of occupied ticks in storage  </tr>
<tr><td>19</td><td>uint64</td><td> 64 /  0</td><td>2</td><td>poolv3::nftv3item_counter</td><td>Pool is also an NFT collection. So this is the counter of currently minted positions  </tr>
<tr><td>20</td><td>uint64</td><td> 64 /  0</td><td>2</td><td>poolv3::nftv3items_active</td><td>Pool is also an NFT collection. Number of unburnt items  </tr>
<tr><td>21</td><td>addr</td><td> 267 /  0</td><td>2</td><td>poolv3::admin_address</td><td>Admin address. Can init pool  </tr>
<tr><td>22</td><td>addr</td><td> 267 /  0</td><td>2</td><td>poolv3::controller_address</td><td>Controller address. Can change fee and lock and unlock pool  </tr>
<tr><td>23</td><td>addr</td><td> 267 /  0</td><td>21</td><td>poolv3::jetton0_minter</td><td>Address of the 0 token minter. Beta only.  </tr>
<tr><td>24</td><td>addr</td><td> 267 /  0</td><td>21</td><td>poolv3::jetton1_minter</td><td>Address of the 1 token minter. Beta only.  </tr>
<tr><td>25</td><td>dict</td><td> 0 /  1</td><td>3</td><td>poolv3::ticks_dictionary</td><td>Storage of the ticks   </tr>
<tr><td>26</td><td>code</td><td> 0 /  1</td><td>4</td><td>poolv3::accountv3_code</td><td>Pool knows how to create user accounts to store fund pairs for a particular user  </tr>
<tr><td>27</td><td>code</td><td> 0 /  1</td><td>4</td><td>poolv3::position_nftv3_code</td><td>Pool knows how to create user position. So it stores it's code  </tr>
<tr><td>28</td><td>cell</td><td> 0 /  1</td><td>4</td><td>poolv3::nftv3_content</td><td>packed metadata that would be given to nft that corresponds to nft colletion  </tr>
<tr><td>29</td><td>cell</td><td> 0 /  1</td><td>4</td><td>poolv3::nftv3item_content</td><td>packed metadata that would be given to nft that corresponds to the position  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 873 | 150 | 
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
 
(slice, slice,  slice, slice, slice, slice, int, int, int, int, int, int, int, int, int, int, int, int,  int, int, int, int, int  ) getPoolStateAndConfiguration ()
 
  
  Returns pool state and configuration

  * @return0 Router address  
  * @return1 Admin address  
  * @return2 Jetton 0 Wallet address. Wallet is owned by the Router
  * @return3 Jetton 1 Wallet address. Wallet is owned by the Router
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

 
## getChildContracts
 
(cell, cell) getChildContracts ()
 
 
  returns code of the child contracts *deprecated*

  * @return1 code of the account contract
  * @return2 code of the nft position contract  
 
## getTickInfosFrom
 
(tuple, tuple) getTickInfosFrom (int key, int amount, int dir)
 
  
   Returns amount ticks starting form key (non-inclusive)
   0 forward 
   1 backward

  * @param key
  * @param amount
  * @param dir

  * @return0
  * @return1
 
## getCollectedFees
 
(int, int) getCollectedFees (int tickLower, int tickUpper, int posLiquidityDelta, int posFeeGrowthInside0X128, int posFeeGrowthInside1X128)
 
  
  Predicts how much fees can a position collect
  
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
 
  
  Computes estimates fot the mint

  * @param  tickLower
  * @param  tickUpper
  * @param  liquidity
  
  * @return0 amount of jetton0 needed to mint the position 
  * @return1 amount of jetton1 needed to mint the position
  * @return2 error code that shows if the basic checks for the mint would succseed 
   
 
## getSwapEstimate
 
(int, int) getSwapEstimate (int zeroForOne, int amount, int sqrtPriceLimitX96)
 
  
  Computes estimates fot the swap

  * @param  zeroForOne
  * @param  amount
  * @param  sqrtPriceLimitX96
  
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
 
  
  In accordance with TEP-62

  * @return0   
 
## get_pool_data
 
(int, int, slice, slice, int, int, int, slice, int, int) get_pool_data ()
 
  
  In accordance with ston.fi interface

  * @return0   
 