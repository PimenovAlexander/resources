---
icon: shuffle
---

# Swap calculation

### AMM Base

Algebra AMM is based on the same principles as classical CPF-AMM (e.g., UniswapV2). The internal state of AMM as a system must always satisfy a given invariant, which, in the case of CPF-AMM looks like:

$$X \cdot Y = K$$

Where $$X, Y$$ – token (jetton) reserves, $$K$$ - constant. Jettons are conventionally called jetton1 (Y) and jetton0 (X):

**slice\_hash(jetton0\_address) > slice\_hash(jetton1\_address)**.

Uniswap V3 transformed the system to a new form. While preserving the basic invariant, two new values are introduced:

$$\sqrt P$$ is the root of the current price of jetton0 relative to jetton1. In the limit, at infinitesimal values, it reduces to $$\sqrt {\Delta Y / \Delta X}$$.

$$L$$ is [liquidity](liquidity-and-positions.md)

At each point in time, the state of the AMM as a dynamic system is given by these two values. Interactions with AMM Algebras during swaps, adding or removing liquidity change the state of the AMM, affecting these values. Only one of these values changes at any given time, with the change in the price root and liquidity being related by the following formulas:

$$\Delta \sqrt P = \Delta Y / L$$

$$\Delta ({1 \over \sqrt P}) = \Delta X / L$$

Where $$X$$ is the balance change of jetton0 at the pool, and $$Y$$is the balance change of jetton1 at the pool. Thus, a change in the price root "generates" the movement of jettons in and out of the pool. Basically, swap can be described as a process of "movement" of the price to some value.

However, the peculiarity of AMM Algebra is concentrated liquidity - in the course of price movement liquidity can increase or decrease due to crossing of position boundaries of liquidity providers. For this purpose, [a tick mechanism](ticks.md) is implemented\\

### Swap as price movement from tick to tick

#### Main cycle

In each tick is recorded the value of $$L$$, which should be added/subtracted to the liquidity, depending on the direction in which the tick crosses (left to right or right to left). Due to this, the whole swap process is represented as a price movement towards the next active tick (to the right or left depending on the swap direction - zeroToOne to the left/down, oneToZero to the right/up). If the price reaches an active tick, the current liquidity changes and the movement continues to the next tick:

1. Find out the current price ($$P_{current}$$)
2. Find out the price on the next active tick ($$P_{next}$$)
3. Calculate jetton entry and exit for price movement to ($$P_{next}$$)
4. If jetton input/output does not exceed the input conditions, then cross the tick and change the current liquidity value. $$P_{current} := P_{next}$$. Return to step 2.
5. If there is unspent jetton input/output left, determine to what price the current price can be moved to using the remaining stock.

So as it is possible, the price moves from tick to tick, and then its movement stops somewhere between ticks, depending on the set number of tokens on the input or output of the swap.

#### Limit on the number of tokens (amountRequired)

#### The limit on the number of tokens(jettons) at swap can be set in two ways, which are conventionally called exactInput and exactOutput:

**exactInput** - swap should use no more input jettons than specified.

_(due to TON architecture v1 TONCO doesn't support exactOutput - the swap should output no more jettons than specified.)_

#### Restriction on price changes

Additionally, the parameter limitSqrtPrice is taken into account when calculating the swap, which imposes a limit on the possible price movement - if the price reaches this value, the swap is stopped.

This parameter allows to simplify some scenarios of AMM usage, including arbitrage or jetton price adjustment.

### Fees calculation

#### Fees

The protocol is supported by the use of liquidity (jettons), which is provided by liquidity providers. One of the incentives for providing liquidity is to receive fees, which is charged to traders at each swap. To simplify the calculation, fees are always taken in the input jetton.

Fees value is set as a fraction of the sum of jettons at the input of the swap and is a constant during the swap.

$$F_{amount}^x = X_{input} \cdot fee / 10000$$

$$F_{amount}^y = Y_{input} \cdot fee / 10000$$

At each iteration of the main swap cycle, the price movement is calculated by taking into account the need to use an appropriate share of the input jettons to pay fees to liquidity providers.

The accumulator value of the following form is used to distribute the accumulated fees among active liquidity providers:

$$totalFeeGrowth = F_{amount} / L$$

Each swap, or rather each iteration within the main swap cycle, increases this accumulator by a new summand. Further, liquidity providers get their share of the collected commissions by multiplying their contribution to liquidity by the corresponding delta of the values of this accumulator (more detailed description in [the article about positions and liquidity](liquidity-and-positions.md)).

#### Protocol Fee

The protocol also takes a portion of the fees for itself. This share of fees in TONCO AMM is called **Protocol Fee**. Protocol fee is calculated as a fraction of also within each iteration of the main swap cycle:

$$protocolFeeAmount = F_{amount} \cdot protocolFee / 10000$$

#### Token(Jetton) Delta Math

This library implements jetton delta calculations using the formulas mentioned above:

$$\Delta \sqrt P = \Delta Y / L$$

$$\Delta ({1 \over \sqrt P}) = \Delta X / L$$

Accordingly, X is called jetton0Delta and Y is called jetton1Delta.

To obtain deltas, the formulas are converted to the following form:

$$\Delta Y = (\sqrt P_1 - \sqrt P_0 )\cdot L$$

$$\Delta X = (\sqrt P_0 - \sqrt P_1 )\cdot {L \over ( \sqrt P_0 \cdot \sqrt P_1)}$$

#### Price Movement Math

A key component of this library is the movePriceTowardsTarget method, which provides the parameter values that result from moving the price to a given target under specified conditions.

Given an input/output jetton constraint, target price, current price, commission value, and swap direction, the method determines:

* The price value resulting from the price movement
* Required number of input jettons (including fee)
* Number of output jettons
* Number of tjettonsokens collected as fee
