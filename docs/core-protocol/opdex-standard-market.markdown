---
layout: page
title: OpdexStandardMarket
parent: Core Protocol
nav_order: 8
---

<small>Inheritance: [OpdexMarket](#references), [IOpdexStandardMarket](#references)</small>

Standard markets create and manage liquidity pools based on the market's configurations set during the time of creation.

Configurations that can be set at the time of market creation include the authorization of pool creators, liquidity providers or traders. The transaction fee of a standard market can also be set from 0% to 1% in 0.1% increments.

The transaction fee applies to all liquidity pools within the market, specifically incurred on swap transactions.

A market fee can also be set during market creation where the owner of the market will collect 1/6 of all transaction fees and the other 5/6 will be collected by liquidity providers.

All configurations of a standard market can only be set at the time of creation and are not able to be changed after with the exception of the address of the market owner.

{: .info }
This smart contract is derived from the [market](opdex-market) smart contract where inherited properties, methods, logs, models and references are detailed.

## Constructor

```csharp
public OpdexStandardMarket(ISmartContractState state,
                           uint transactionFee,
                           Address owner,
                           bool authPoolCreators,
                           bool authProviders,
                           bool authTraders,
                           bool enableMarketFee) : base(state, transactionFee)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |
| `Address` | owner | The address of the market's staking token. |
| `bool` | authPoolCreators | Flag to authorize liquidity pool creators. |
| `bool` | authProviders | Flag to authorize liquidity pool providers. |
| `bool` | authTraders | Flag to authorize traders. |
| `bool` | enableMarketFee | Flag to determine if 1/6 of transaction fees should be collected by the market. |

---

## References

#### OpdexMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexMarket.cs" target="_blank">Github</a>

#### OpdexStandardMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexStandardMarket.cs" target="_blank">Github</a>

#### IOpdexStandardMarket Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Markets/IOpdexStandardMarket.cs" target="_blank">Github</a>
