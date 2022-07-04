---
layout: page
title: OpdexMiningPool
parent: Core Protocol
nav_order: 5
---

<small>Inheritance: [SmartContract](#references), [IOpdexMiningPool](#references)</small>

A smart contract used to deposit liquidity pool tokens to mine for new staking tokens. Each mining pool contract is tied to an individual staking pool in the staking market.

Users provide liquidity to staking pools in return for liquidity pool tokens. Liquidity pool tokens are temporarily held in mining pools to mine for new staking tokens. When a mining pool runs out of tokens to mine, mining is paused until the mining pool is nominated again to receive more tokens to be mined.

Rewarded tokens from mining and the liquidity pool tokens used to mine can be withdrawn at any time and are not under any time locks. Once mined tokens are collected, the received tokens are intended to be staked in staking pools to participate in the governance protocol.

Staking rewarded tokens from mining in liquidity pools nominates the liquidity pool for more mining. At the end of each nomination period, the top 4 staking pools by weight will have liquidity mining enabled. Mine to receive staking tokens, stake to nominate a pool for more mining.

Liquidity mining is optional for liquidity providers. Users are encouraged to provide liquidity to staking pools and mine new staking tokens to help create better price performance during swaps.

## Constructor

```csharp
public OpdexMiningPool(ISmartContractState state,
                       Address minedToken,
                       Address stakingToken) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | minedToken | The smart contract address of the token being mined. |
| `Address` | stakingToken | The smart contract address of the token used for mining. |

---

## References

#### OpdexMiningPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexMiningPool.cs" target="_blank">Github</a>

#### IOpdexMiningPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexMiningPool.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
