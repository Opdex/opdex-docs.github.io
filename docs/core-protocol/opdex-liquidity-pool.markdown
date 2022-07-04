---
layout: page
title: OpdexLiquidityPool
parent: Core Protocol
nav_order: 1
---

<small>Abstract Class</small>
<small>Inheritance: [SmartContract](#references), [IOpdexLiquidityPool](#references)</small>

An abstract smart contract class that is inherited by [standard pool](opdex-standard-pool) and [staking pool](opdex-staking-pool). Provides a base class sharing similar properties and methods between the two different liquidity pool types.

## Constructor

```csharp
protected OpdexLiquidityPool(ISmartContractState state,
                             Address token,
                             uint transactionFee) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | token | The address of the SRC token in the pool. |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |

---

## References

#### OpdexLiquidityPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexLiquidityPool.cs" target="_blank">Github</a>

#### IOpdexLiquidityPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexLiquidityPool.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
