---
layout: page
title: Token Swaps
parent: Knowledge Center
nav_order: 2
---

Token swaps allow users swap between any two compatible tokens within Opdex liquidity pools.

For example, a user swaps from USDT to CRS tokens.

<iframe width="100%" style="aspect-ratio: 16/9;" src="https://www.youtube-nocookie.com/embed/2koXqQVgPZE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## Transaction Fees

Token swaps incur a transaction fee, that is set by the market and paid by users as they execute token swaps. Different markets can have different transaction fees from 0% to 1% but the primary Opdex staking market has a fixed transaction fee of 0.3%.

Fees paid from token swaps are collected by liquidity providers and in staking pools, partially to stakers.

See [Providing Liquidity](doc:providing-liquidity) and [Staking](doc:staking) for more details.

## Allowances

When selling SRC-20 tokens, an allowance will need to be approved in order for the transaction to succeed. Allowances are approvals given to smart contracts so they can spend a number of tokens on your behalf, as long as you've signed and submitted the transaction.

For example, USDT is an SRC-20 token, so to sell 10 will require an allowance approval of at least 10 tokens.

## Price Impact

Price impact is the measurement of how much a swap will impact the liquidity pool's reserves and underlying token prices.

A higher price impact means that the trade is large and is shifting the underlying token price ratios at a higher rate. The lower the price impact, the more efficient the swap is.

## Slippage Tolerance

Slippage tolerance is similar to price impact but is used to buffer added price impact during high volume periods. For example, a swap transaction has an expected price impact of 1% and is submitted. If other transactions front run and change the price by more than 1%, the transaction will fail. Adding a slippage tolerance of 3% would mean that the transaction should still succeed as long as the expected result is less than the 3% change allowed by the user.

## Deadlines

Deadlines allow for transactions to fail if they are not executed by a certain block number. For example, if the network is severely congested, transactions may not be mined for a few blocks as they wait to be mined. Adding a deadline allows the transaction to fail safely if if is not mined by the submitted deadline.
