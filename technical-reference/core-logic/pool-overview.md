# 🧺 Pool overview

### One pool for each pair

The liquidity pool is a key component of the TONCO protocol. This smart contract implements the most important functions that provide swaps, liquidity management, and other protocol functionality.

Due to the nature of TON the actual pool contract controls consistency of the math and data structures for the swaps, mints and burns. Actual funds are on the wallets that belong to router contract. Pool only monitors and updates the reserves that belong to it.

For each pair of tokens(jettons) in the TONCO, one unique pool is created. This approach minimizes liquidity fragmentation, simplifies the construction of optimal routes for swaps, and simplifies liquidity management for liquidity providers. _(TONCO v1 Due to TON non-atomic architecture multihop swaps are not yet supported.)_

The address of each pool is determined deterministically by the mechanism using token(jetton) wallet addresses owned by the router as salt. Generally this address depends on 5 parameters:

* Jetton0/Jetton1 wallet address&#x20;
* Router Address&#x20;
* Subcontracts code
  * &#x20;NFT position contract code&#x20;
  * User account code&#x20;
* Actual pool code

Liquidity pools are not intended for direct use by ordinary users - for convenient use of the protocol functionality, users use peripheral contracts that implement additional calculations and security checks. Pool won't accept majority of the operations form any user, with the exception of the burn start.

### Requirements for tokens

The liquidity pool expects the token to be TEP-74/89 standard. In addition, there are the following limitations:&#x20;

1. The Algebra TONCO does not support tokens that can arbitrarily reduce balances at addresses.
2. The Algebra TONCO does not guarantee full functionality when using tokens whose total supply exceeds $$2^{128} -1$$(maximum value uint128).
3. The Algebra TONCO does not guarantee full performance with tokens that have the ability to selfdestruct or upgradeable code.

### Pool Description

The main purpose of the pool is to enable swaps using concentrated liquidity.Below is a high-level description of the main functionality implemented in the pool.

#### Token(Jetton) Swap

The main task of AMM is to provide token swap. Swapping in TONCO is performed using concentrated liquidity technology. More detailed description of the swap process [the article about swap calculations](swap-calculation.md).

In Algebra Integral there are two variants of swap - with prepayment and with payment after settlement.

Due to TON architecture jettons for the swap and gas are first sent by the user and after swap computations are finished they would get the swap result, initial jetton change(if any) and excess gas back. Front-end helps to compute how many jettons and gas should be sent.

#### Liquidity positions

Users can provide tokens as liquidity at some given price range. A liquidity position, as long as it is active, receives a portion of the fees that are collected on each swap. See [the article on liquidity and liquidity positions](liquidity-and-positions.md) for a more detailed description of the logic and calculations.

Basic actions with liquidity positions:

* `mint` - increase liquidity in the position by providing jetton in exchange for NFT. Whoever calls the method must provide jettons to the pool. _(v1 TONCO so far doesn't allow to add more jettons to existing position)_\
  When adding liquidity, the tickspacing restriction is taken into account - you cannot add liquidity to a position if the indices of the corresponding ticks are not divisible by `tickSpacing`.
* `burn` - reduction of liquidity in the position. As a result of liquidity reduction user receives both - the part of the pool reserves and the fees collected by the position since position creation or previous burn
* `burn0, collect` - the user receives the tokens that are recorded as a fee in the corresponding liquidity position.

#### Pool Customization

Each Algebra Integral pool has a number of parameters that can be changed by the protocol team, DAO or authorized persons. Persons authorized to change pool (each has own limitations) :

1. DEX Administrator (Router administrator)
2. Pool Administrator
3. Pool Controller

Customizable parameters:

* `lock/unlock` - way to temporary stop the mint and swap in the pool. burn/burn0 operation are always available&#x20;
* `protocolFee` - share of the collected fee that is available to administrator for the needs of the protocol.
* `tickSpacing` - limit on ticks that can be used as boundaries of the liquidity position. New liquidity can only be added to positions whose upper and lower ticks are divided wholly by `tickSpacing`. This means that if `tickSpacing` is equal to 60, then only every 60th tick (... -60, 0, 60 ...) can be used as a new liquidity position boundary.
* `fee` - the commission for swaps in the pool can be changed manually or from authorized backend.
