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

## Properties

| Type | Property | Description |
| `Address` | MiningGovernance | The address of the governance contract responsible for distributing tokens to be mined. |
| `Address` | StakingToken | The contract address of the liquidity pool token used for mining. |
| `Address` | MinedToken | The contract address of the token being mined. |
| `ulong` | MiningPeriodEndBlock | The end block of the mining period. |
| `UInt256` | RewardRate | The amount of tokens mined per block. |
| `ulong` | MiningDuration | The number of blocks mining is scheduled for. |
| `ulong` | LastUpdateBlock | The last block where a transaction occurred causing reward rate calculations. |
| `UInt256` | RewardPerStakedTokenLast | The latest calculated amount of rewards per full liquidity pool token used for mining from the last time any miner executed a mining action. |
| `UInt256` | TotalSupply | The total supply of liquidity pool tokens currently mining. |
| `bool` | Locked | Contract reentrancy locked status. |

---

## Methods

### Get Stored Reward Per Staked Token

Retrieves the last calculated reward per full liquidity pool token for the provided address from the last action executed by the miner.

```csharp
public UInt256 GetStoredRewardPerStakedToken(Address miner);
```

#### Parameters

| Type | Property | Description |
| `Address` | miner | The miner's address. |

#### Returns

| Type | Property | Description |
| `UInt256` | amount | The last calculated amount of tokens earned per liquidity pool token used for mining. |

---

### Get Stored Reward

Retrieves the last calculated reward amount from state for a provided address.

```csharp
public UInt256 GetStoredReward(Address miner);
```

#### Parameters

| Type | Property | Description |
| `Address` | miner | The address of the miner to check the rewards for. |

#### Returns

| Type | Property | Description |
| `UInt256` | reward | The number of earned tokens from mining. |

---

### Get Balance

Returns the balance of liquidity pool tokens used for mining from a provided address.

```csharp
public UInt256 GetBalance(Address miner);
```

#### Parameters

| Type | Property | Description |
| `Address` | miner | The address of the miner to check the balance of. |

#### Returns

| Type | Property | Description |
| `UInt256` | balance | The miner's balance of liquidity pool tokens used for mining. |

---

### Latest Block Applicable

Returns either the current block number or the last block of the mining period, whichever is less.

```csharp
public ulong LatestBlockApplicable();
```

#### Returns

| Type | Property | Description |
| `ulong` | latestBlockApplicable | The latest applicable block number. |

---

### Get Reward For Duration

Calculates the total tokens being distributed for the current mining period.

```csharp
public UInt256 GetRewardForDuration();
```

#### Returns

| Type | Property | Description |
| `UInt256` | reward | The number of tokens distributed throughout the entire current mining period. |

---

### Get Reward Per Staked Token

Calculates and returns the current rewards per full liquidity pool token used to mine.

```csharp
public UInt256 GetRewardPerStakedToken();
```

#### Returns

| Type | Property | Description |
| `UInt256` | reward | Amount of earnings per token used to mine based on the current state of the pool. |

---

### Get Mining Rewards

Calculates and returns the current amount of mined tokens earned by the miner.

```csharp
public UInt256 GetMiningRewards(Address miner);
```

#### Parameters

| Type | Property | Description |
| `Address` | miner | The miner's address to check earned rewards for. |

#### Returns

| Type | Property | Description |
| `UInt256` | reward | The amount of tokens earned through mining. |

---

### Start Mining

Using an allowance, transfer liquidity pool tokens to mine with for rewarded tokens.

```csharp
public void StartMining(UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of liquidity pool tokens to mine with, requires an allowance approval. |

---

### Collect Mining Rewards

Collects and transfers earned rewards to miner while continuing to mine.

```csharp
public void CollectMiningRewards();
```

---

### Stop Mining

Withdraws the specified amount of liquidity pool tokens mining and collects rewards.

```csharp
public void StopMining(UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of tokens to withdraw from the mining pool. |

---

### Notify Reward Amount

Hook used to notify this mining pool of rewarded funding. Sets reward rates and mining periods.

```csharp
public void NotifyRewardAmount(UInt256 reward);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | reward | The amount of tokens rewarded to the mining pool for mining. |

---

## Logs

### Start Mining Log

Emitted when a user starts mining.

| Index | Type | Property | Description |
| ✅ | `Address` | Miner | The address of the miner. |
| ❎ | `UInt256` | Amount | The amount of liquidity pool tokens being added. |
| ❎ | `UInt256` | TotalSupply | The total amount of tokens mining in the pool. |
| ❎ | `UInt256` | MinerBalance | Updated balance of the miner in the pool. |

---

### Stop Mining Log

Emitted when a user stops mining.

| Index | Type | Property | Description |
| ✅ | `Address` | Miner | The address of the miner. |
| ❎ | `UInt256` | Amount | The amount of liquidity pool tokens being withdrawn. |
| ❎ | `UInt256` | TotalSupply | The total amount of tokens mining in the pool. |
| ❎ | `UInt256` | MinerBalance | Updated balance of the miner in the pool. |

---

### Collect Mining Rewards Log

Emitted when a miner collects earned rewards.

| Index | Type | Property | Description |
| ✅ | `Address` | Miner | The address of the miner collecting rewards. |
| ❎ | `UInt256` | Amount | The amount of collected rewards. |

---

### Enable Mining Log

Emitted when a mining pool is notified of newly rewarded tokens to enable or continue mining.

| Index | Type | Property | Description |
| ❎ | `UInt256` | Amount | The amount of received tokens to be mined. |
| ❎ | `UInt256` | RewardRate | The amount of tokens to be mined per block. |
| ❎ | `ulong` | MiningPeriodEndBlock | The block height identifying when the mining period will end. |

---

## References

#### OpdexMiningPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexMiningPool.cs" target="_blank">Github</a>

#### IOpdexMiningPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexMiningPool.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
