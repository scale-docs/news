---
layout: post
title: Trading (For Devs & Bots!)
date: 2023-10-23 00:00:00
description: Technical guide for developers and bots to integrate with Scale.Farm's Solidly-based router and swap functions.
tags: docs trading developers bots integration technical
categories: docs
---

# Trading (For Devs)

you would have to also change a few small things in your logic there, since Scale.Farm is not a Uniswap v2 fork but instead based on Andre's Solidly
if you are launching an 'exotic' token like Auto-LP or Auto-sell/buy on Transfers, please keep the following things in mind:

please use the ABI from our existing router.. find the right address in the Contract Address sections


1. createPair function

    it needs 3 things instead of 2 needed by univ2. We need token0, token1 and **stable**, which is a boolean (true/false) to mark whether this pool has both same-priced tokens. use false here.

2. addLiquidity

    same as above, it needs an extra parameter, **stable**. use false there

3. swap* functions

    swaps use a `struct Route` to define the path, unlike the `address[]` array used in uniswap.

uniswap way for 3 token path (2 hops, 1->2 & 2->3): ```arm
address[] memory mypath = new address[](3);
mypath[0] = 0xtoken1;
mypath[1] = 0xtoken2;
mypath[2] = 0xtoken3;
uniRouter.swapExactTokensForTokens(
  amountIn,
  amountoutMin,
  mypath,
  to,
  deadline
);
```
in Scale.Farm, we represent hops instead of path, using Route object.
length is 2 (hops) instead of 3 (path addresses)```js
struct Route {
  address from;
  address to;
  bool stable;
}
Route[] memory myroute = new Route[](2); //note length is 2
myroute[0] = Route({from:0xtoken1, to: 0xtoken2, false});
myroute[1] = Route({from:0xtoken2, to: 0xtoken3, false});
EqualRouter.swapExactTokensForTokens(
  amountIn,
  amountoutMin,
  myroute, //same but using route not path
  to,
  deadline
);
```

also, consider using the *SupportingFeeOnTransferTokens methods instead of plain methods please for higher compatibility

other than these smol bits, the process should be similar to how you trade on uniswap v2

