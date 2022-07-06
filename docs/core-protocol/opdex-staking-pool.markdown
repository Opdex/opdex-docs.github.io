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

## Properties

| Type | Property | Description |
| `Address` | StakingToken | The address of the pool's staking token. |
| `Address` | MiningPool | The mining pool associated with this staking pool contract. |
| `UInt256` | TotalStaked | The total amount of staked tokens. |
| `UInt256` | StakingRewardsBalance | The balance of liquidity pool tokens to be collected by stakers. |
| `UInt256` | ApplicableStakingRewards | The latest fees that haven't been accounted for when calculating reward per staked token. |
| `UInt256` | RewardPerStakedTokenLast | The amount of liquidity pool token rewards per full token staked from the last time any staker executed a staking action. |

---

## Methods

### Get Stored Reward Per Staked Token

Retrieves the last calculated reward per staked token stored during the last action executed by the staker.

```csharp
public UInt256 GetStoredRewardPerStakedToken(Address staker);
```

#### Parameters

| Type | Property | Description |
| `Address` | staker | The address of the staker. |

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The amount of rewards per staked token from the last action taken by the staker. |

---

### Get Stored Reward

Retrieves the last calculated reward per staked token stored during the last action executed by the staker.

```csharp
public UInt256 GetStoredReward(Address staker);
```

#### Parameters

| Type | Property | Description |
| `Address` | staker | The address of the staker. |

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The last calculated reward amount for the staker. |

---

### Get Reward Per Staked Token

Retrieves the current reward per full token staked based on the most recent calculation of total staking pool rewards.

```csharp
public UInt256 GetRewardPerStakedToken();
```

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The amount of rewarded tokens per full staked token. |

---

### Get Staked Balance

Retrieves the amount of tokens staked for an address.

```csharp
public UInt256 GetStakedBalance(Address staker);
```

#### Parameters

| Type | Property | Description |
| `Address` | staker | The address of the staker to check the balance of. |

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The amount of staked tokens. |

---

### Get Staking Rewards

Retrieves the amount of earned rewards based on the most recent calculation of total staking pool rewards.

```csharp
public UInt256 GetStakingRewards(Address staker);
```

#### Parameters

| Type | Property | Description |
| `Address` | staker | The address of the staker to check the reward balance of. |

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The amount of earned rewards. |

---

### Start Staking

Stakes tokens in a liquidity pool. Users must have an approved allowance of the amount they wish to stake.

Using the `TransferFrom` IStandardToken256 method implementation, the users allowance is sent to the liquidity pool for staking.

Staking in a liquidity pool makes a call to the staking token and mining governance contracts to nominate the liquidity pool for mining, recording the pool's updated staking weight.

```csharp
public void StartStaking(UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of tokens to stake. |

---

### Collect Staking Rewards

Collects earned staking rewards while continuing to stake.

By setting the `liquidate` parameter to true, earned liquidity pool tokens will be burned in the same transaction, returning an equal ratio of the pool's reserve tokens.

```csharp
public void CollectStakingRewards(bool liquidate);
```

#### Parameters

| Type | Property | Description |
| `bool` | liquidate | When true, liquidates earned LP tokens in exchange for an equal ratio of the pool's reserve tokens. |

---

### Stop Staking

Stops staking in the liquidity pool. The staker's tokens are returned and earned rewards are collected in the form of LP tokens.

Exiting the staking pool makes a call to the staking token and mining governance contracts to nominate the liquidity pool for mining, recording the pool's updated staking weight.

By setting the `liquidate` parameter to true, earned liquidity pool tokens will be burned in the same transaction, returning an equal ratio of the pool's reserve tokens.

```csharp
public void StopStaking(UInt256 amount, bool liquidate);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of tokens to stop staking with. |
| `bool` | liquidate | When true, liquidates earned LP tokens in exchange for an equal ratio of the pool's reserve tokens. |

---

## Logs

### Start Staking Log

Emitted when a user starts staking tokens.

| Index | Type | Property | Description |
| ✅ | `Address` | Staker | The address of the staker. |
| ❎ | `UInt256` | Amount | The amount of tokens added to the staking position. |
| ❎ | `UInt256` | TotalStaked | The total amount of tokens being staked in the pool. |
| ❎ | `UInt256` | StakerBalance | Updated staking balance for the staker. |

---

### Stop Staking Log

Emitted when a user stops staking tokens.

| Index | Type | Property | Description |
| ✅ | `Address` | Staker | The address of the staker. |
| ❎ | `UInt256` | Amount | The amount of tokens removed from the staking position. |
| ❎ | `UInt256` | TotalStaked | The total amount of tokens being staked in the pool. |
| ❎ | `UInt256` | StakerBalance | Updated staking balance for the staker. |

---

### Collect Staking Rewards Log

Emitted when a user collects liquidity pool token staking rewards.

| Index | Type | Property | Description |
| ✅ | `Address` | Staker | The address of the staker collecting rewards from the pool. |
| ❎ | `UInt256` | Reward | The amount of rewarded liquidity pool tokens collected by the staker. |

---

## References

#### OpdexLiquidityPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexLiquidityPool.cs" target="_blank">Github</a>

#### OpdexStakingPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexStakingPool.cs" target="_blank">Github</a>

#### IOpdexStakingPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexStakingPool.cs" target="_blank">Github</a>
