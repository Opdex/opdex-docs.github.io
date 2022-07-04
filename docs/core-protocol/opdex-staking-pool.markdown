---
layout: page
title: OpdexStakingPool
parent: Core Protocol
nav_order: 7
---

<small>Inheritance: [OpdexLiquidityPool](#references), [IOpdexStakingPool](#references)</small>

An extended version of a liquidity pool with added staking capabilities. Staking allows users to transfer and hold designated staking tokens in staking pools to participate in the governance protocol.

By participating in staking, liquidity pools are nominated for the next round of liquidity mining where miners will mine to receive new staking tokens. Liquidity mining helps encourage higher liquidity in pools resulting in better price performance.

In short, by staking in chosen liquidity pools, participants are voting for the staking pool to have liquidity mining enabled in the next round of mining.

Staking tokens are only used for staking in liquidity pools to secure the governance protocol. To incentivize participation, staking pools split the transaction fees from swaps to go partially to stakers and partially to liquidity providers.

Stakers will collect 0.05% and liquidity providers will collect 0.25% of transaction swaps through transaction fees. Staking is not time-locked so tokens can be collected at any time and users can stop staking at any time.

Staking pools distribute fees slightly differently than standard pools. In staking pools using the KLast property of the last transaction (ReserveCrs \* ReserveSrc) the staking pool calculates and inflates the liquidity pool token supply by 0.05% and designates the inflated tokens to be collected by stakers.

Distributing to stakers in this manner allows for only distributing fees to stakers if there are active staking participants. Otherwise, if a staking pool has 0 staking weight, the supply is not inflated and liquidity providers receive all of the transaction fees.

Received tokens through staking are in the form of liquidity pool tokens, which can be liquidated for the pool's reserve tokens, held for liquidity providing, or used in liquidity mining.

See [standard pool](opdex-standard-pool) for more details.

{: .info }
This smart contract is derived from the [liquidity pool](opdex-liquidity-pool) smart contract where inherited properties, methods, logs, models and references are detailed.

## Constructor

```csharp
public OpdexStakingPool(ISmartContractState state,
                        Address token,
                        uint fee,
                        Address stakingToken) : base(state, token, fee)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | token | The SRC token address in the liquidity pool. |
| `uint` | fee | The market transaction fee, 0-10 equal to 0-1%. |
| `Address` | stakingToken | The SRC staking token address. |

---

## References

#### OpdexLiquidityPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexLiquidityPool.cs" target="_blank">Github</a>

#### OpdexStakingPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexStakingPool.cs" target="_blank">Github</a>

#### IOpdexStakingPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexStakingPool.cs" target="_blank">Github</a>
