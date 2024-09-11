# 💰 Liquidity and positions

### Definition of liquidity

As stated in [the article about swap calculations](swap-calculation.md), the internal state of the TONCO AMM at any moment is determined by two values:

$$\sqrt P$$ - the root of the current price of token1(jetton1) relative to token0(jetton0).

$$L$$ - liquidity.

A change in the current price in the pool (via swaps) entails the movement of jettons from / to the pool, with the number of jettons depending on a coefficient called liquidity. Formulas linking token deltas, price change and liquidity value:

$$\Delta \sqrt P = \Delta Y / L$$

$$\Delta ({1 \over \sqrt P}) = \Delta X / L$$

Thus, the liquidity $$L$$ can be defined as a coefficient that determines the "speed" of price change when tokens are swapped. Change of $$\sqrt P$$ (or $$1 / \sqrt P$$, depending on the direction of swap) is inversely proportional to liquidity. This means that the greater the liquidity, the more tokens need to be swapped to move the price by a given value.

### Liquidity position

TONCO is based on the concept of concentrated liquidity. This means that users can provide their tokens as liquidity for swaps at a certain price range. The following describes what this means and how the value of liquidity is related to tokens.

A liquidity position in TONCO is an entity defined by the following parameters:

1. `positionv3::user_address` - Position owner
2. `positionv3::pool_address` - Pool to which the position belongs
3. `positionv3::tickLower` - the tick corresponding to the lowest price at which the liquidity of this position can be used
4. `positionv3::tickUpper` - is the tick corresponding to the highest price at which the liquidity of this position can be used
5. `positionv3::liquidity` - $$\Delta L$$ - liquidity value associated with this position

The liquidity value associated with the position $$\Delta L$$ adds to the global liquidity value when the position becomes active (price inside the specified tick range) and subtracted from the global liquidity value when the position becomes inactive (price outside the specified tick range). These changes take place during the [swap](swap-calculation.md) on the crossing of position-related [ticks](ticks.md).

Thus, the value $$\Delta L$$ must ensure the fulfillment of the formulas from the [liquidity definition](liquidity-and-positions.md#definition-of-liquidity) section on the price range defined by the upper and lower tick of the position. Then the correlation between the number of tokens and $$\Delta L$$ can be obtained as follows. Let:

$$\sqrt {P_{top}}$$ - the price root value corresponding to the upper tick of the position.

$$\sqrt {P_{bottom}}$$ - the price root value corresponding to the lower tick of the position.

$$\sqrt {P_{current}}$$ - the current value of the price root in the pool.

Note that during the upward price movement (zeroToOne = false) the pool buys jetton1 (Y) and sells jetton0 (X). On the other hand, during the downward price movement (zeroToOne = true) the pool buys jetton0 and sells token1.

This means that a liquidity position must, on the one hand, provide enough jetton0 for the sale to move the price up to the $$\sqrt {P_{top}}$$, and, on the other hand, provide for sale a sufficient amount of jetton1 to move the price to $$\sqrt {P_{bottom}}$$ .

#### Correlation between the liquidity value and the amount of tokens(jettons)

Then we can express the correlation between the number of jettons and $$\Delta L$$, if $$\sqrt {P_{current}}$$ is inside the price range of the position:

$$\Delta X = - (1 / \sqrt {P_{top}} - 1 / \sqrt {P_{current}}) \cdot \Delta L$$

$$\Delta Y = - (\sqrt {P_{bottom}} - \sqrt {P_{current}}) \cdot \Delta L$$

If$$\sqrt P_{current}$$ is not inside the position's price range, the amount of jettons associated with the position must cover price movement in one direction only (depending on the position of the current price).

If the current price is higher than the upper price of the position ( $$\sqrt {P_{current}} \ge \sqrt {P_{top}}$$):

$$\Delta X = 0$$

$$\Delta Y = - ( \sqrt {P_{top}} - \sqrt {P_{bottom}}) * \Delta L$$

If the current price is below the price range of the position ( $$\sqrt {P_{current}} \lt \sqrt {P_{bottom}}$$ ):

$$\Delta X = - (1 / \sqrt {P_{top}} - 1 / \sqrt {P_{bottom}}) * \Delta L$$

$$\Delta Y = 0$$

Thus, when creating a position with the given $$\Delta L$$, $$\sqrt {P_{top}}$$, $$\sqrt {P_{bottom}}$$ user must provide $$\Delta X$$jetton0 and $$\Delta Y$$jetton1, calculated according to the above formulas taking into account the current price in the pool.

On the other hand, when withdrawing liquidity, the user should receive$$\Delta X$$jetton0 and $$\Delta Y$$jetton1, also calculated using the same formulas considering the current price and the change in $$\Delta L$$.

### Fee distribution between liquidity positions

For [swaps](swap-calculation.md), the pool deducts a fee that is allocated to the currently active liquidity positions (positions whose liquidity is used for the swap).

Two accumulators of the following form are used for this purpose:

$$totalFeeGrowthJetton0 = \sum F^x_{amount} / L$$

$$totalFeeGrowthJetton1 = \sum F^y_{amount} / L$$

where $$F^{\{x, y\}}_{amount}$$ - the collected amount of fees in jetton0 or jetton1, $$L$$ - the current global value of the fee.

During swap, each iteration of the main loop holds the commission in the input token and increments the value of the corresponding accumulator.

Thanks to this mechanism, it is easy to calculate the share of fees due to each liquidity position. With the help of the ticks mechanism, it is possible to know at any moment what parts of accumulators were added at the moment when the price was between two given ticks. The definition of accumulator increments within the tick range is described in more detail in [the article about ticks](ticks.md).

The corresponding values of accumulators increment are recorded in the position when it was created, let's call them:

$$innerFeeGrowthJetton0_{old}$$ - accumulator increment for jetton0 that occurred between specified ticks

$$innerFeeGrowthJetton1_{old}$$ - accumulator increment for jetton1 that occurred between specified ticks

Then the amount of tokens that correspond to the share of the fees for a liquidity position can be calculated at any time:

$$\Delta fees_x = \Delta L \cdot (innerFeeGrowthJetton0_{new} - innerFeeGrowthJetton0_{old})$$

$$\Delta fees_y = \Delta L \cdot (innerFeeGrowthJetton1_{new} - innerFeeGrowthJetton1_{old})$$

This is followed by an update of $$innerFeeGrowthJetton0_{old}$$ and $$innerFeeGrowthJetton1_{old}$$
