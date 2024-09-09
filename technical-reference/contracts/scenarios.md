# Scenarios

* Scenarios
  * [Deploy Pool](scenarios.md#deploy-pool)
  * [Init Pool](scenarios.md#init-pool)
  * [Mint](scenarios.md#mint)
  * [Swap](scenarios.md#swap)
  * [Burn](scenarios.md#burn)

## Scenarios

### Deploy/Init Pool

Pool deployment is triggered by administrator of the AMM (also the administrator of the router contract). Pool deployment leads to the processing of the POOL\_INIT operation inside pool contract.

This operation is used both - during the initial deployment of the pool and it pool administrator wants to change some crucial parameters. 
Pool deployment consists of two stages - forming and sending state_init that holds
  * Router address
  * Jetton0/Jetton1 wallet addresses (these are attached to the router)
  * Account contract code
  * Position NFT contract code

Next newly created pool would only accept messages from the router and admin. 

<figure><img src="../../images/init.svg" alt="" width="400"><figcaption></figcaption></figure>

### Mint

<figure><img src="../../images/mint.svg" alt="" width="800"><figcaption></figcaption></figure>

### Swap

<figure><img src="../../images/swap.svg" alt="" width="800"><figcaption></figcaption></figure>

### Burn

<figure><img src="../../images/burn.svg" alt="" width="400"><figcaption></figcaption></figure>
