# 📏 Ticks

### Ticks

The entire price space is divided into sections using special cut-offs called ticks.

<figure><img src="../../.gitbook/assets/ticks0.png" alt=""><figcaption></figcaption></figure>

The ticks are distributed logarithmically: the tick with index i corresponds to the price:

$$P(t_i) = 1.0001^i$$

Inside the segment defined by two neighboring ticks (often this segment is also called a tick), the AMM behaves like a regular CPF-AMM (like UniV2). When the price crosses a tick, the value of active liquidity may change (if the boundary of someone's position is crossed).

For this reason, when making swaps, you should be able to find the next active tick (a tick that is the boundary of a position).

#### Storing ticks in TON Contract

To simplify navigation through ticks during swaps and for other purposes, active ticks are organized in a dict() with 24-bit signed ints used as keys.

Because dict() is internally stored as a tree pool always has quick access to the information about which ticks are currently the next and previous active ticks. So when swapping, it is easy to get information about which tick a particular iteration of the swap is up to.

### Determination of fee increment within the range of ticks

Ticks play an important role in allocating the commission between liquidity positions. The accumulator values described in [the article on liquidity and positions](liquidity-and-positions.md) are used for this purpose:

$$totalFeeGrowthJetton0 = \sum F^x_{amount} / L_t$$

$$totalFeeGrowthJetton1 = \sum F^y_{amount} / L_t$$

Two additional values are stored in each tick that correspond to the accumulator increments "outside" the ticks: `outerFeeGrowth0Jetton`, `outerFeeGrowth1Jetton`.

These values have a relative character and are set at the moment of tick initialization according to the following rule: it is presumed that the entire "increment" of the commission accumulator occurred **below** this tick. For this reason, the values are initialized as follows:

If the tick to be initialized is less than or equal to the global tick at the moment$$(tick \le currentTick)$$:

$$outerFeeGrowth0Jetton = totalFeeGrowthJetton0$$

$$outerFeeGrowth1Jetton = totalFeeGrowthJetton1$$

On the other hand, if the tick to be initialized is above the current global tick $$(tick \gt currentTick)$$, then the corresponding values are initialized to zero:

$$outerFeeGrowth0Jetton = 0$$

$$outerFeeGrowth1Jetton = 0$$

Later on, at each tick crossing these values are updated according to the following rule:

$$outerFeeGrowth0Jetton_{new} = totalFeeGrowthJetton0 - outerFeeGrowth0Jetton_{old}$$

$$outerFeeGrowth1Jetton_{new} = totalFeeGrowthJetton1 - outerFeeGrowth1Jetton_{old}$$

This ensures that knowing the current global tick, it is possible at any time to determine what jetton increment has occurred "on the other side" since the tick was initialized:

<figure><img src="../../.gitbook/assets/ticks3.png" alt="" width="563"><figcaption><p>outerFeeGrowth - commission accumulator increment "on the other side" from tick N</p></figcaption></figure>

At the same time, the pool possesses the accumulator values `totalFeeGrowthJetton0` and `totalFeeGrowthJetton1`, which contains the total commission increment for the entire time of the pool's existence.

<figure><img src="../../.gitbook/assets/ticks4.png" alt="" width="563"><figcaption><p>totalFeeGrowth - total fee / L growth for the entire pool lifetime</p></figcaption></figure>

Due to these values, it is easy to calculate the value of the commission increment that occurred within a given range of ticks after their initialization.

If $$tick_K \le currentTick \lt tick_N$$:

<figure><img src="../../.gitbook/assets/ticks5.png" alt="" width="563"><figcaption><p>innerFeeGrowth - increment of accumulator fee / L from the moment of ticks initialisation</p></figcaption></figure>

If $$tick_K \lt tick_N \le currentTick$$:

$$innerFeeGrowth_{K, N} = outerFeeGrowth_N - outerFeeGrowth_K$$

If $$currentTick \lt tick_K \lt tick_N$$:

$$innerFeeGrowth_{K, N} = outerFeeGrowth_K - outerFeeGrowth_N$$

**Thus**, using the above formulas for `innerFeeGrowth` it is possible to know the accumulator increment $$\sum F^{x, y}_{amount} / L_t$$ within the range specified by any two active ticks at any time. Distribution of commission among liquidity positions is performed by tracking the change of `innerFeeGrowth` for a liquidity position:

$$\Delta fee_{position} = \Delta L_{position} * \Delta innerFeeFrowth _{position}$$

The corresponding bit at the root is 1 if the corresponding second-level word has at least one active bit.

The maximum allowed tick is **887272**

The minimum allowed tick is -**887272**.

Thus, there can be (887272 + 887272 + 1).

.
