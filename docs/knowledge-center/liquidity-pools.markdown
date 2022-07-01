---
layout: page
title: Liquidity Pools
parent: Knowledge Center
nav_order: 1
---

Liquidity pools are token pairings where liquidity providers lend tokens. Providers deposit and lock liquidity within the pool so that token swaps can utilize the existing liquidity reserves. There will be CRS reserves and SRC reserves within a liquidity pools and the ratio of the two tokens is what makes up the token price in AMM based DeFi protocols.

Because liquidity pools are based on the token reserves ratios, there is no order book and there are no limit orders. By looking at the ratio of the token reserves, the price of a token can be calculated. For example, a liquidity pool with 10 CRS and 20 SRC tokens in reserves would make 1 CRS equal to 2 SRC tokens or 1 SRC token equal to 0.5 CRS tokens.

![Liquidity Pool View](a76db9b-LiquidityPool.jpg)

## Transaction Types

Liquidity pools primarily help facilitate swap and liquidity provisioning based transaction types.

## Token Swaps

Token swaps are trades from one token to another. They incur a transaction fee that gets paid to liquidity providers and is the primary function of a liquidity pool.

For more information, see [Token Swaps](token-swaps)

## Liquidity Provisioning

Liquidity provisioning is the act of providing or removing liquidity from within a liquidity pool. These tokens provided by users are what are used to swap from one token to another.

For more information, see [Providing Liquidity](providing-liquidity)

## Staking Pools

In staking markets, liquidity pools have added functionality to support governance staking.

Staking is the act of depositing governance tokens (ODX) to vote to enable liquidity mining for the pool. User's participating in staking will collect 0.05% of the total 0.3% of the pool's transaction fees.

<!-- For more information, see [Staking](doc:knowledge-center-staking) -->
