---
layout: page
title: OpdexMarket
parent: Core Protocol
nav_order: 2
---

<small>Abstract Class</small>
<small>Inheritance: [SmartContract](#references), [IOpdexMarket](#references)</small>

An abstract smart contract class that is inherited by [standard market](opdex-standard-market) and [staking market](opdex-staking-market). Provides a base class sharing similar properties and methods between the two different market types.

## Constructor

```csharp
protected OpdexMarket(ISmartContractState state, uint transactionFee) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |

---

## References

#### OpdexMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexMarket.cs" target="_blank">Github</a>

#### IOpdexMarket Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Markets/IOpdexMarket.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
