---
description: >-
  Dive into how exactly TONCO is transforming liquidity management on TON with
  the cutting-edge concentrated liquidity solutions in our technical overview
  below
cover: >-
  ../.gitbook/assets/roogai._Display_a_series_of_vertical_bars_each_representing_dif_cc0e851e-7a2a-4d31-b16c-e4c8ada1f0ad.png
coverY: 0
layout:
  cover:
    visible: true
    size: hero
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🫗 Bringing Concentrated Liquidity to TON

## The Limitations of AMMs on TON <a href="#da80" id="da80"></a>

After closely examining the designs of Ethereum and TON, it becomes clear that TON introduces unique innovations, particularly in how smart contracts operate. TON has carved out a distinct position in the blockchain landscape by enabling a high volume of transactions without compromising quality. This duality reflects both its strength — offering immense potential for the evolution of DeFi — and its challenge, as existing DeFi approaches need to be reimagined to fully harness TON’s capabilities.

Automated Market Makers (AMMs) like Uniswap v2 revolutionized the DeFi space with their use of the XYK model (x\*y=k), which maintains balance within liquidity pools by ensuring the total value of one token equals the total value of the other, regardless of price fluctuations. This model was a breakthrough in its time, providing a simple yet effective way to manage liquidity.

On the TON blockchain, a similar approach is emerging, though it diverges significantly from the early EVM methods. While it adopts some common and straightforward principles, TON’s version is poised to evolve beyond the conventional models, potentially reshaping the future of DeFi on this new platform.

**The currently available DEXes on TON are the following:**

* DeDust.io
* Ston.fi
* Megaton.fi
* TON Swap
* Toncoin.com
* Ton Diamonds DEX
* Storm Trade
* MARS DEX
* swap.coffee
* Snapster Trading
* Kibble Exchange
* Polkaswap DEX
* DEXTON
* MyTonSwap

And much more. Most of them are running on the traditional XYK model, with no DEXes running on the so-called V3 concept.

However, this approach had significant drawbacks. In the XYK model, liquidity is spread across all possible price ranges. This distribution often results in lower trading fees and higher slippage for LPs since much of the liquidity remains unused. For instance, in pools consisting of stablecoins like DAI and USDC, most trading occurs within a narrow price range close to parity. Yet, the XYK model inefficiently allocates liquidity across the entire price spectrum, leading to suboptimal returns for LPs.

## V3 Revolution: The New Liquidity Pooling Approach <a href="#a4d3" id="a4d3"></a>

That’s where the V3 concept comes in, introducing Concentrated Liquidity, Dynamic Fees, and other groundbreaking features. It’s a true revolution in the world of AMMs, bringing forth innovative solutions like Automated Liquidity Management, Liquid Staking, modular architecture possibilities, and more.

This entire V3 AMM model has been introduced on EVM networks, but neither the V3 nor V4 models have been implemented on non-EVM networks like the TON chain. This means Tonco is pioneering this technology on TON. However, we’re not just replicating what’s available on other networks; we’re adapting and enhancing these capabilities specifically for DeFi on TON. Let’s delve into what Concentrated Liquidity is all about.

## Enter TONCO: Concentrated Liquidity <a href="#d6cb" id="d6cb"></a>

TONCO introduces a paradigm shift with Concentrated Liquidity — a feature that significantly enhances capital efficiency. Unlike the traditional XYK model, TONCO allows LPs to allocate liquidity within specific price intervals — price range — creating what’s known as a concentrated liquidity position. This approach enables LPs to open multiple positions in a pool, each tailored to their desired price ranges and risk profiles, resulting in deeper liquidity.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*tJzaS_QkiWJoNseK" alt="" height="655" width="700"><figcaption></figcaption></figure>

Users can either select a predetermined price range from the list of presets or manually set the range by choosing the minimum and maximum prices as they prefer.

<figure><img src="https://miro.medium.com/v2/resize:fit:700/0*FkS2PbBBx9A9sFwj" alt="" height="659" width="700"><figcaption></figcaption></figure>

When the market price falls within an LP’s chosen range, their liquidity is used for trades, earning them a share of the trading fees proportional to their contribution within that range. As the price fluctuates, different LPs’ liquidity is utilized, optimizing capital use and ensuring liquidity is available where it’s most needed.

This innovation offers significant benefits:

* **Higher Capital Efficiency:** LPs can concentrate their funds within specific price ranges, maximizing their fee earnings.
* **Reduced Slippage:** Traders benefit from deeper liquidity in the most relevant price intervals, reducing the risk of slippage.
* **Flexible Risk Management:** LPs can adjust their positions to minimize or even prevent impermanent loss.
* **Customizable Price Ranges:** LPs have the flexibility to select price intervals they believe will see the most trading activity, enabling them to maximize potential returns.
* **Increased Fee Revenue:** The concentration of liquidity within chosen price bands can lead to higher fee generation, as trades within these bands are more frequent and substantial.

**For those who have experienced high impermanent loss on platforms like Uniswap V3, TONCO’s concentrated liquidity model offers a more robust solution, empowering you to minimize risks and maximize returns: such an approach increases DEX’s capital efficiency UP TO 95% compared to traditional V2 AMMs!**

## Dynamic Fees: Adapting to Market Conditions <a href="#id-4468" id="id-4468"></a>

TONCO’s dynamic fee structure is another standout feature that sets it apart from traditional AMMs. Unlike Uniswap V3, which offers fixed fees across multiple pools, TONCO employs a dynamic, volatility-based fee model that adjusts based on market conditions, such as volatility and pool volume.

That means that in TONCO, LPs don’t need to choose between multiple pools with different fee structures. Instead, they benefit from a single pool where fees are dynamically adjusted, simplifying the process and enhancing profitability.

## Streamlined Farming with TONCO <a href="#id-6e9a" id="id-6e9a"></a>

TONCO takes farming to the next level by integrating it directly into the platform. Unlike Uniswap, where users need to interact with external smart contracts to farm tokens, TONCO offers an all-in-one solution. Users can stake their extra tokens in liquidity pools and earn rewards seamlessly within the platform.

This streamlined approach simplifies the farming process, making it easier for users to generate yield and boost their overall returns without the hassle of navigating multiple platforms.

## Modular Approach: V4 AMM is coming? <a href="#id-567f" id="id-567f"></a>

TONCO is designed to soon integrate dynamic fees, modules, limit orders, trading discounts, and KYC — all in the form of V4 plugins.

The entire V4 AMM concept is based on a Modular Architecture with custom plugins, reinventing the way DEXes operate. This approach splits the previously monolithic infrastructure into two key components: the Core Codebase and Plugins. These plugins are crucial for enhancing the efficiency, security, and adaptability of DEXes, making it a strong competitor to Uniswap V4.

TON’s design is markedly different from EVM approaches, allowing for the creation of nearly any plugin, unlike EVM chains. By compromising on decentralization, we can also reimagine the Modular AMM approach. Security is ensured by keeping plugin execution off-chain and handled on the backend.

## Conclusion: The Future of DeFi on TONCO <a href="#id-70fc" id="id-70fc"></a>

TONCO is more than just a DEX; it’s a complete ecosystem designed to elevate the DeFi experience on the TON chain. With innovations like concentrated liquidity, dynamic fees, and built-in farming, TONCO offers unparalleled flexibility, efficiency, and profitability for both LPs and traders.

Whether you’re looking to minimize impermanent loss, maximize trading fees, or simply enjoy a more seamless farming experience, TONCO has the tools you need to succeed in the fast-evolving world of DeFi. Stay tuned as we continue to push the boundaries of what’s possible in decentralized finance.
