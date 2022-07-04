---
layout: page
title: OpdexStakingMarket
parent: Core Protocol
nav_order: 6
---

<small>Inheritance: [OpdexMarket](#references), [IOpdexStakingMarket](#references)</small>

The staking market contract is fully public and instead of creating standard liquidity pools it creates staking liquidity pools with a set transaction fee equal to 0.3%.

Staking in liquidity pools is used to secure the mining and token governance protocol. Staking in liquidity pools automatically nominates the liquidity pool for liquidity mining and the top 4 liquidity pools by staking weight at the end of each nomination period will have liquidity mining enabled.

Liquidity mining allows liquidity providers to mine for new staking tokens and encourages providers to deposit liquidity to pools in order to mine. Mining helps ensure higher liquidity amounts resulting in better price performance for traders.

To incentivize stakers to participate in governance, stakers will receive partial transaction fees according to their weight staked compared to the entire staking weight of the liquidity pool. Of the 0.3% transaction fee for each swap in an active staking pool, 0.25% transaction fee is collected by liquidity providers and the other 0.5% is collected by stakers.

{: .info }
This smart contract is derived from the [market](opdex-market) smart contract where inherited properties, methods, logs, models and references are detailed.

## Constructor

```csharp
public OpdexStakingMarket(ISmartContractState state,
                          uint transactionFee,
                          Address stakingToken) : base(state, transactionFee)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |
| `Address` | stakingToken | The address of the market's staking token. |

---

## Properties

| Type | Property | Description |
| `Address` | StakingToken | The address of the staking token used within staking pools. |

---

## References

#### OpdexMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexMarket.cs" target="_blank">Github</a>

#### OpdexStakingMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexStakingMarket.cs" target="_blank">Github</a>

#### IOpdexStakingMarket Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Markets/IOpdexStakingMarket.cs" target="_blank">Github</a>
