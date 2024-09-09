# 🧺 Pool overview

### One pool for each pair

The liquidity pool is a key component of the Algebra Integral protocol. This smart contract implements the most important functions that provide swaps, liquidity management, flashloans and other protocol functionality.

For each pair of tokens in the Algebra protocol, one unique pool is created. This approach minimises liquidity fragmentation, simplifies the construction of optimal routes for swaps, and simplifies liquidity management for liquidity providers.

The address of each pool is determined deterministically by the `create2` mechanism using token addresses as salt.

Liquidity pools are not intended for direct use by ordinary users - for convenient use of the protocol functionality, users use peripheral contracts that implement additional calculations and security checks.

### Requirements for tokens

The liquidity pool expects the token to be ERC20 standard. In addition, there are the following limitations:

1. The Algebra TONCO does not support tokens that can arbitrarily reduce balances at addresses.
2. The Algebra TONCO does not guarantee full functionality when using tokens whose total supply exceeds $$2^{128} -1$$(maximum value uint128).
3. The Algebra TONCO does not guarantee full performance with tokens that have the ability to selfdestruct or upgradeable code.

### Pool Description

The main purpose of the pool is to enable swaps using concentrated liquidity.

Below is a high-level description of the main functionality implemented in the pool.

#### Token Swap

The main task of AMM is to provide token swap. Swapping in Algebra Integral is performed using concentrated liquidity technology. More detailed description of the swap process in [the article about swap calculations](swap-calculation.md).

In Algebra Integral there are two variants of swap - with prepayment and with payment after settlement.

**Swap with payment after computation (`swap`)** is the main variant that should be used for most token pairs. First, the calculations required for swap are performed on the specified parameters, then the user receives the output tokens, after which he must pay for the input tokens inside the callback (`algebraSwapCallback`).

**`SwapWithPaymentInAdvance`** is an additional variant of swap that can be useful for some classes of tokens (e.g., tokens that charge a transfer fee). First, the user must send the required number of input tokens in a callback (algebraSwapCallback), then calculations are performed, then the user receives the output tokens.

#### Liquidity positions

Users can provide tokens as liquidity at some given price range. A liquidity position, as long as it is active, receives a portion of the fees that are collected on each swap. See [the article on liquidity and liquidity positions](liquidity-and-positions.md) for a more detailed description of the logic and calculations.

Basic actions with liquidity positions:

* `mint` - increase liquidity in the position by providing more tokens. If the corresponding position does not exist, it is created. Whoever calls the method must provide tokens to the pool\
  When adding liquidity, the tickspacing restriction is taken into account - you cannot add liquidity to a position if the indices of the corresponding ticks are not divisible by `tickSpacing`.
* `burn` - reduction of liquidity in the position. As a result of liquidity reduction user receives both - the part of the pool reserves and the fees collected by the position since position creation or previous burn
* `burn0, collect` - the user receives the tokens that are recorded as a fee in the corresponding liquidity position.

#### Pool Customization

Each Algebra Integral pool has a number of parameters that can be changed by the protocol team, DAO or authorized persons. Persons authorized to change pool parameters (see [the AlgebraFactory and roles article](../../core-logic/broken-reference/) for more information on roles and access control):

1. DEX Administrator
2. Pool Controller

Customizable parameters:

* `communityFee` - share of the collected commissions that is sent to AlgebraCommunityVault for the needs of the protocol.
* `tickSpacing` - limit on ticks that can be used as boundaries of the liquidity position. New liquidity can only be added to positions whose upper and lower ticks are divided wholly by `tickSpacing`. This means that if `tickSpacing` is equal to 60, then only every 60th tick (... -60, 0, 60 ...) can be used as a new liquidity position boundary.
* `fee` - if dynamic commission mode is not enabled, the commission for swaps in the pool can be changed manually.
