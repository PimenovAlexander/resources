# 🧺 Pool overview

### One pool for each pair

The liquidity pool is a key component of the TONCO protocol. This smart contract implements the most important functions that provide swaps, liquidity management, and other protocol functionality.

Due to the nature of TON, actual pool contract controls the consistency of the math and data structures for swaps, mints and burns. Actual funds are stored in the wallets that belong to the router contract. The pool only monitors and updates the reserves that belong to it.

For each pair of tokens(jettons) in TONCO, one unique pool is created. This approach minimizes liquidity fragmentation, simplifies the construction of optimal routes for swaps, and streamlines liquidity management for liquidity providers. _(TONCO v1: Due to TON's non-atomic architecture, multihop swaps are not yet supported.)_

The address of each pool is calculated deterministically by a mechanism using token(jetton) wallet addresses owned by the router as salt. Generally this address depends on 5 parameters:

* Jetton0/Jetton1 wallet address
* Router Address
* Subcontracts code
  * NFT position contract code
  * User account code
* Actual pool code

Liquidity pools are not intended for direct use by ordinary users. For convenient use of the protocol functionality, users use peripheral contracts that implement additional calculations and security checks. Pool won't accept the majority of the operations from any user, with the exception of the initiation of burn.

### Requirements for tokens

The liquidity pool expects the token to adhere to the TEP-74/89 standard. Additionally, there are the following limitations:

1. The Algebra TONCO does not support tokens that can arbitrarily reduce balances at addresses.
2. The Algebra TONCO does not guarantee full functionality when using tokens whose total supply exceeds $$2^{120} -1$$(maximum value uint120).
3. The Algebra TONCO does not guarantee full performance with tokens that have the ability to self-destruct or use upgradeable code.

### Pool Description

The primary purpose of the pool is to enable swaps using concentrated liquidity.Below is a high-level description of the main functionality implemented in the pool.

#### Token(Jetton) Swap

The main task of AMM is to facilitate token swaps. Swapping in TONCO is performed using concentrated liquidity technology. A more detailed description of the swap process can be found in [the article about swap calculations](swap-calculation.md).

Due to the architecture of TON, jettons for the swap and gas are first sent by the user. And after swap computations are finished they receive swap result, initial jetton change(if any) and any excess gas back. The front-end helps to compute how many jettons and gas should be sent.

#### Liquidity positions

Users can provide tokens as liquidity within a specific price range. A liquidity position, as long as it is active, receives a portion of the fees that are collected on each swap. For a more detailed description of the logic and calculations see [the article on liquidity and liquidity positions](liquidity-and-positions.md)&#x20;

Basic actions with liquidity positions include:

* `mint` - increase liquidity in the position by providing jetton in exchange for NFT. Whoever calls the method must provide jettons to the pool. _(v1 TONCO currently doesn't allow adding more jettons to an existing position)_\
  When adding liquidity, the tickspacing restriction is taken into account. You cannot add liquidity to a position if the indices of the corresponding ticks are not divisible by `tickSpacing`.
* `burn` - reduce liquidity in the position. As a result of liquidity reduction the user receives both - a part of the pool reserves and the fees collected by the position since its creation or previous burn
* `burn0, collect` - the user receives the jettons recorded as a fee in the corresponding liquidity position.

#### Pool Customization

Each TONCO AMM pool has a number of parameters that can be changed by the protocol team, DAO or authorized persons. Persons authorized to change the pool parameters (each has their own limitations) :

1. DEX Administrator (Router administrator)
2. Pool Administrator
3. Pool Controller

Customizable parameters:

* `lock/unlock` - a way to temporary stop the mint and swap in the pool. burn/burn0 operations are always available
* `protocolFee` - the share of the collected fee that is available to the administrator for the needs of the protocol.
* `tickSpacing` - a limitation on ticks that can be used as the boundaries of the liquidity position. New liquidity can only be added to positions whose upper and lower ticks are divisible  by `tickSpacing`. For example if `tickSpacing` is equal to 60, then only every 60th tick (... -60, 0, 60 ...) can be used as a new liquidity position boundary.
* `fee` - the commission for swaps in the pool can be adjusted manually or from authorized backend.
