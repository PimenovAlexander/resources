# 🏦 Reserves

### Reserves definition

A reserves mechanism was introduced to track increases in pool balances. Reserves reflect the last known pool balances - after the last major pool action has been performed (`mint`/`burn`/`swap`/`collect`/`flash`). Immediately after interaction with the pool, under normal conditions, reserves should have the following values:

$$reserveJetton0 = balanceJetton0$$

$$reserveJetton1 = balanceJetton1$$

Synchronization of reserves and balances after interaction with the pool occurs based on the expected change in balances - using the number of tokens sent or requested by the pool. For example, sending tokens to a user ($$jettonAmount^{\{0, 1\}}$$) causes the next change in the corresponding reserve:

$$reserveJetton^{\{0,1\}}_{new} = reserveJetton^{\{0,1\}}_{old} - jettonAmount^{\{0,1\}}$$

On the other hand, when $$jettonAmount^{\{0, 1\}}$$ is received, the reserve is increased accordingly:

$$reserveJetton^{\{0,1\}}_{new} = reserveJetton^{\{0,1\}}_{old} + jettonAmount^{\{0,1\}}$$

Thus, the value of the reserve at any given moment reflects the number of jettons the pool **expects** to have on its balance.

### Reserves synchronization

At the beginning of the main pool interactions, it is checked whether the pool balances match the expected reserve values. In case of discrepancy, the corresponding jetton surplus (jettonDelta) is calculated and distributed among active liquidity positions as additional fees if the current liquidity $$L$$ is not zero:

$$jetton0Delta = balanceJetton0 - reserveJetton0$$

$$jetton1Delta = balanceJetton1 - reserveJetton1$$

$$totalFeeGrowth0_{new} = totalFeeGrowth_{old} + jetton0Delta / L$$

$$totalFeeGrowth1_{new} = totalFeeGrowth_{old} + jetton1Delta / L$$

If current liquidity is zero ($$L = 0$$), no synchronization occurs until liquidity becomes non-zero.

Thanks to this mechanism it became possible to support jetton rebase by the TONCO protocol - the pool can detect an increase in jetton balances due to external reasons and distribute the excess jettons among active liquidity providers.





### Limits

The reserve value for each of the jettons is limited to the maximum value of the data type `uint128`: $$2^{120} - 1$$. This restriction allows us to minimize the cost of gas when interacting with the pool.

However, this restriction means that TONCO does not guarantee full pooling performance when using jettons with totalSupply greater than the maximum value of `uint120`. For most pool interactions, if the pool interaction results in the reserve exceeding the maximum allowed value, the coins would be returned to the user

### Pending Protocol Fee

To minimize the number of unnecessary transfers, Protocol Fee is not sent to the Pool Admin at each swap. Instead, the accumulated part of the commissions is recorded in the variables `poolv3::collectedProtocolFee{0,1}`.

Accumulation and sending of Protocol Fee is taken into account in tracking pool reserves.
