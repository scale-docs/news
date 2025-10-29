---
layout: post
title: Liquidity Pools
date: 2023-10-06 00:00:00
description: Deep dive into Scale.Farm's liquidity pool system, fees, and different pool types from v1 to v5.
tags: docs liquidity pools amm fees stable variable
categories: docs
---

# Liquidity Pools


Scale.Farm is an AMM which uses the traditional liquidity pool system to create tradable pairs. The system we have deployed is, in our opinion, an improvement on the uniswap system that uses fees to incentivize these pools. Instead, we use a continuous loop system to utilise tokens tied to the fees to incentivize our system. This system creates an equalized situation which can allow for token utility and growth.

The reason we have selected this model is to empower the use of our token and create a system where people lock the token for the benefits it provides.

The incentive system will provide a steady loop of incentives to ensure a low slippage trade can be completed with fair fee charges.

## Fees

In 2022, Scale.Farm V1 set the fees at a rate of 0.2% per trade for volatile pairs and 0.02% for stable pairs.

After many months of observation and testing, in 2023 we moved to a dynamic, "editable fee" structure, which would let us tweak the fee % per pool, with a default fee of 1% for vAMM & 0.04% for sAMM on Fantom.

In 2024, we invented a fully-dynamic fee which enabled us to tweak fees automatically with pinpoint precion and zero offchain latency. This fee structure supports the promotion and retention of liquidity, which is essential for maintaining low slippage trades during various phases of the market.

Most of the AMM's in the market use a similar fee amount to reward Liquidity Providers. From our detailed research, we have found that this fee is necessary to sustain a healthy environment for attracting liquidity. One should not focus only on the fee amount, but the efficiency of the trade that they can complete on the AMM. For this reason, the Scale.Farm model is the first of its kind to create a sustainable dex that supports the equilibrium required to maintain an effective balance to LP providers and token holders for stable and volatile assets.

## Types of Pools

Scale.Farm v1 comes with 2 basic types of liquidity pools

### Stable Pools

Stable pools are designed for assets which have little to no volatility. This means that the formula used for pricing the assets allows for low slippage, even on large traded volumes.

x³y + xy³ = k

### Variable Pools

Variable pools are designed for assets with high price volatility. These pools use a generic AMM formula.

x × y = k

## Scale.Farm v2
Scale.Farm v2 is capable of hosting multiple types of liquidity

### Derivatives Trading Vault
Scale.Farm's v2 also brings in support for Vault receipts of a Derivatives platform, such as the EQUITY token from our [Equity DEX](../Equity/Equity), as well as other partnering Perpetuals Protocols / Orcale-driven AMMs, such as GMX & Woofi like forks, such as Morphex & Equity on Fantom Opera.

## Scale.Farm v3

### Concentrated Liquidity
Scale.Farm v2 can natively support Conentrated Liquidity for staking, voting, trading & market-making. It uses wrappers from AUtomated Liquidity Management Protocols (A.L.M.) that tokenize Concentrated Liquidity into stakeable ERC20 receipts. We support [Thick CL DEX](ftm.guru/docs/thick) & the [E3 DEX](https://ftm.guru/docs/e3).

## Scale.Farm v4
Scale.Farm v4 expands upon the supported types to bring more new types under the roof.

### Lending Markets
Through [Eliteness Lending Market Aggregator](https://ftm.guru/docs/elma), Scale.Farm can provide liquidity incentives to Lenders, paid out as EQUAL emissions. In return, the partner protocols send Interest and/or other yield to Scale.Farm as bribes.

## Scale.Farm v5
We shift our tagline to being a Liquidity Market, and not merely a dex.

### Multi-token pools, Stable & MetaStable pools
A weighted mix of upto 8 tokens inside a single pool. Equi-weighted pools containting upto 4 Correlated Assets, such as a pool of multiple Stablecoins or a pool of a Base asset and its Syntheic Derivates, such as Liquid-Staking Derivatives or Interest-Bearing Assets. Brought in using protocols such as Balancer v1/v2/v3, Beets or forks thereof.

## Manual CL
Rewards for Non-fungible Concentrated Liquidity Position holders based on customizable Key-Performance Metrics to pintpoint and reward the Liquidity Providers that prove to be the most useful for our short-term & long-term goals.

## Options
Coming Soon!

## Permissionless Pools
Coming Soon!