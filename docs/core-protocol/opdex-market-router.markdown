---
layout: page
title: OpdexMarketRouter
parent: Core Protocol
nav_order: 4
---

<small>Inheritance: [SmartContract](#references), [IOpdexRouter](#references)</small>

A router contract acts as the primary entry point for exchange based, multi-step transactions within a single market. This contract is responsible for routing quotes, swaps, adding or removing liquidity based transactions.

Transactions are quoted, prerequisite allowance transfers are made and hops between pools are possible during a single transaction. This allows for optimistic transfers and ultimately, savings in gas costs.

Because these transaction types require multiple steps and optimistic transfers, users should not call liquidity pool smart contracts directly. The above mentioned transaction types should be called through this contract only or a trusted third party smart contract integrated with Opdex protocols.

Third party smart contracts can be written to perform these multi-step transactions for integrations directly with pools. These types of integrations can take advantage of flash swaps (borrowing), arbitrage opportunities and other use cases.

## Constructor

```csharp
public OpdexRouter(ISmartContractState state,
                   Address market,
                   uint transactionFee,
                   bool authProviders,
                   bool authTraders) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | market | The address of the market associated. |
| `uint` | transactionFee | 0-10 transaction fee equal to 0-1%. |
| `bool` | authProviders | Flag indicating if liquidity providers should be authorized. |
| `bool` | authTraders | Flag indicating if traders should be authorized. |

---

## References

#### OpdexRouter Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Routers/OpdexRouter.cs" target="_blank">Github</a>

#### IOpdexRouter Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Routers/IOpdexRouter.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
