## position_nft

* [Description](#description)
* [Data Storage](#data-storage)
* [Interface](#interface)
* [Messages](#messages)

# Description 

This is a modifed NFT contract to store user position

# Data Storage 
<table data-full-width="true">
<thead>
<tr><th width="70">Index</th><th width="100">Type</th><th width="100">Size (b/r)</th><th width="58">Cell</th><th width="280">Name</th><th>Description</th></tr>
</thead>
<tbody>
<tr><td>1</td><td>uint64</td><td> 64 /  0</td><td>1</td><td>positionv3::index</td><td>The position number. Also the nft index  </tr>
<tr><td>2</td><td>addr</td><td> 267 /  0</td><td>1</td><td>positionv3::pool_address</td><td>Address of the pool that created this NFT  </tr>
<tr><td>3</td><td>addr</td><td> 267 /  0</td><td>1</td><td>positionv3::user_address</td><td>Address of the user ton wallet that currently owns the position  </tr>
<tr><td>4</td><td>cell</td><td> 0 /  1</td><td>1</td><td>positionv3::content</td><td>NFT metadata that contains image url, name and description packed in standart format  </tr>
<tr><td>5</td><td>uint128</td><td> 128 /  0</td><td>1</td><td>positionv3::liquidity</td><td>Position liquidity  </tr>
<tr><td>6</td><td>int24</td><td> 24 /  0</td><td>1</td><td>positionv3::tickLower</td><td>Positioni lower tick number  </tr>
<tr><td>7</td><td>int24</td><td> 24 /  0</td><td>1</td><td>positionv3::tickUpper</td><td>Positioni upper tick number  </tr>
<tr><td>8</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>positionv3::feeGrowthInside0LastX128</td><td>Fees collected before the position was opened or updated for jetton0 (in pool terms)  </tr>
<tr><td>9</td><td>uint256</td><td> 256 /  0</td><td>11</td><td>positionv3::feeGrowthInside1LastX128</td><td>Fees collected before the position was opened or updated for jetton1 (in pool terms)  </tr>
</tbody>
</table>


# Cells 
| Name |   Size  |   Free  |
| ---  |  ---    |  ---    |
| 1  | 774 | 249 | 
| 11  | 512 | 511 | 

# Interface 
## getPoolAddress
 
(slice) getPoolAddress ()
 
 
  This function returns pool address that created this Position NFT

  * @return0 address in question 
 
## getUserAddress
 
(slice) getUserAddress ()
 
 
  This function returns user address that owned created this Position NFT

  * @return0 address in question 
 
## getPositionInfo
 
(int, int, int, int, int) getPositionInfo ()
 
 
  This function returns data stored in Position NFT and is related to the position

  * @return0 liquidity that this position owns
  * @return1 lower tick of the position
  * @return2 upper tick of the position
  * @return3 fee growth of jetton0 in the given range at moment of the creation or latest collect of the NFT position
  * @return4 fee growth of jetton1 in the given range at moment of the creation or latest collect of the NFT position
 
## get_nft_data
 
(int, int, slice, slice, cell) get_nft_data ()
 
 
  This function returns data of this Position NFT that is related to NFT as TEP-62
 
# Messages 

## POSITIONNFTV3_POSITION_INIT
Opcode : **0xd5ecca2a** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  |  | 
| user_address | Address() |  | 
| liquidity | Uint(128) |  | 
| tickLower | Int(24)   |  | 
| tickUpper | Int(24)   |  | 
| feeGrowthInside0LastX128 | Uint(256)  |  | 
| feeGrowthInside1LastX128 | Uint(256)  |  | 
| nftIndex | Uint(64),Indexer |  | 
| jetton0Amount | Coins(),Indexer |  | 
| jetton1Amount | Coins(),Indexer |  | 
| tick | Int(24),Indexer |  | 

## POSITIONNFTV3_POSITION_BURN
Opcode : **0x46ca335a** 



| Mnemonic | Type | Description |
| --- | --- | --- |
| op | Uint(32) op |  | 
| query_id | Uint(64)  |  | 
| nft_owner | Address() |  | 
| liquidity2Burn | Uint(128) |  | 
| tickLower | Int(24)   |  | 
| tickUpper | Int(24)   |  | 
| feeGrowthInside0LastX128 | Uint(256)  |  | 
| feeGrowthInside1LastX128 | Uint(256)  |  | 

## NFT_TRANSFER,POSITIONNFTV3_NFT_TRANSFER
Opcode : **0x5fcc3d14** 



| Mnemonic | Type | Description |
| --- | --- | --- |
