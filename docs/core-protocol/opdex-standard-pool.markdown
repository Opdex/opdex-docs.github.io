---
layout: page
title: OpdexStandardPool
parent: Core Protocol
nav_order: 9
---

<small>Inheritance: [OpdexLiquidityPool](#references), [IOpdexStandardPool](#references)</small>

A standard liquidity pool that holds reserves of CRS and a single SRC20 token type used for swapping tokens against.

Each liquidity pool manages its own liquidity pool SRC20 token which are minted or burned as users add or remove liquidity from the pool. LP tokens represent providers share of reserves in the liquidity pool.

Traders swap tokens against the existing pool's reserves, the higher the amount of reserves, the more stable the price is during swaps.

Standard pools, created within standard markets, may have authorizations in place for pool creators, liquidity providers or traders depending on the market's configurations that creates the standard pool.

The standard market contract that creates standard liquidity pools also determines the transaction fees of created liquidity pools. The fees can range from 0% to 1% and once set, cannot be changed.

In a standard pool, 100% of the transaction fees are collect by liquidity providers at the time of withdrawing provided liquidity from the pool.

{: .info }
This smart contract is derived from the [liquidity pool](opdex-liquidity-pool) smart contract where inherited properties, methods, logs, models and references are detailed.

## Constructor

```csharp
public OpdexStandardPool(ISmartContractState state,
                         Address token,
                         uint transactionFee,
                         bool authProviders,
                         bool authTraders,
                         bool marketFeeEnabled) : base(state, token, transactionFee)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | token | The SRC token address in the liquidity pool. |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |
| `bool` | authProviders | Flag to authorize liquidity providers or not. |
| `bool` | authTraders | Flag to authorize traders or not. |
| `bool` | marketFeeEnabled | Flag determining if 1/6 of transaction fees are collected by the market owner. |

---

## Properties

| Type | Property | Description |
| `Address` | Market | The address of the market the pool is assigned to. |
| `bool` | AuthProviders | Flag describing whether `Mint`, `Burn`, `Sync`, or `Skim` transactions are authorized or not. |
| `bool` | AuthTraders | Flag describing whether `Swap` transactions are authorized or not. |
| `bool` | MarketFeeEnabled | Flag indicating if the market owner collects 1/6 of all transaction fees. |

---

## References

#### OpdexLiquidityPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexLiquidityPool.cs" target="_blank">Github</a>

#### OpdexStandardPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexStandardPool.cs" target="_blank">Github</a>

#### IOpdexStandardPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexStandardPool.cs" target="_blank">Github</a>
