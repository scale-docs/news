---
layout: post
title: Listing Special Tokens on Scale.Farm
date: 2023-10-19 00:00:00
description: Guide for listing exotic tokens like rebasing, fee-on-transfer, and ERC404 tokens on Scale.Farm.
tags: docs special tokens exotic rebasing fee-on-transfer
categories: docs
---

# List Special tokens

## Compatibility
Our Classic Volatile Pools fully support Trading and Liquidity additions of all Special tokens such as Rebasing tokens, Elastic Supply tokens, ERC404s, Fee-on-Transfer tokens, etc.

However, to earn the Trade Fees generated from such tokens, you must **exempt** the trade fees generated from double taxation.

Exotic tokens must make the `pairFees` address of all their pools exempt of any taxes or rebases, or else Liquidity Providers might not be able to claim the trade-fee rewards!

## Finding pair's "Fees" contract"
You can easily find it on the liquidity pool's ftmscan page on the 'Read Contract' tab under the **`fees`** heading.

## Effects of exemptions
Exempting the pair `Fees` address will not impact the transfer taxes taking place during trades or any other interactions with the liquidity pool. Your rebases or taxes will still work as expected. **This step only removes the taxes/rebases from the trade-fee portion, not the actual trade volume!**

## Gauges & Bribes
Scale.Farm's Gauges & Bribes are the First DEX to natively support Exotic tokens. **Gauges & Bribes Dont require special exemptions.**

But make sure to exempt the pair `Fees` address from any transfer-tax, rebase (positive or negative), Reflection rewards or other special features. If you dont, you wont be able to enjoy all the features of Scale.Farm!

## Correcting previously generated fees
In case you had not whitelisted the Fees address, it could have accrued a deficit of tokens. As in, if it ad expected 100 tokens and got only 99, it would think it has 100 but while transferring fees during fees claims, it would encounter an error and revert, since it has just 99 and not 100.

Solution to this is to simply sending it the remaining tokens.
- First of all, make sure it has been exempted or whitelisted by the Token's team or creator.
- Check how much of Fees was collected ever since the creation.
- Now transfer it the amount that might have been charged as a trade fees.
> If it was sent 500 tokens but received only 450 tokens (10% tax), then you need to send it the 50 tokens Plus some more to account for the taxation during your transfer to it. In this case, you should send it
> 50 (=10% of 500, the total lifetime fees)
> + 5 (=10% of 50, the deficit)
> + 1 (a little bit more)
> **= 56 tokens**

# Changes needed to your Token's Smart Contract Code
Scale.Farm is not a UniswapV2 fork, and its routers and pairs are slightly different.

you would need to make these small adjustments to your contract if your Token involves deployment to a LP on the DEX during its Constructor() call

the changes required are minimal:

## 1. addLiquidity() function
in here, we have one additional parameter called `stable` which you must include alongside the token0 and token1 addresses

## 2. swapExact<ETH/Token>For<Tokens/ETH>SupportingFeesOnTransferTokens
in here, Uniswapv2 forks use the `address[] memory path`.
in case of Scale.Farm, you should use `Route[] memory path`, where Route is a `Struct`:
```
Struct Route {
  address from;
  addresss to;
  bool stable;
}
```

## 3. Token Name and Symbol
Instead of a public string variable, we recommend making it a constant or a function. dont make it an immutable variable, make it a constant instead, or use a function like

```
function name() external pure returns(string memory) { return "My Token!"; }
function symbol() external pure returns(string memory) { return "MYTOKEN"; }
```

that should be all thats needed.

# MAX SUPPLY LIMIT for Suppported Tokens
another thing to note is that your token supply should be a max of 36 digits including the decimals. for example EQUAL has 18 decimals and a max supply of 21 million = 18+8 = 24 digits.
if you want more token supply, then we suggest reducing decimals to 9 or 6 and then you can have a token with 999 999 999 999 999 999 999 999 999 999 max supply with 6 decimal precision (so 30+6=36)
That is, make `YourToken.totalSupply()` less than solidity's `type(uint224).max` to be safe.