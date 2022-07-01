---
layout: page
title: OpdexMiningGovernance
parent: Governance Protocol
nav_order: 2
---

<small>Inheritance: [SmartContract](#references), [IOpdexMiningGovernance](#references)</small>

A governance smart contract responsible for holding tokens to be distributed to miners based on a mining block schedule through liquidity mining. Liquidity pools chosen to enable mining is based on a nomination by staking weight.

Every staking action in a liquidity pool automatically nominates the pool for liquidity mining in the next round. Liquidity pools make a contract call to the Mined Token contract which calls this Mining Governance contract to record the pool's staking weight.

At the end of each nomination period, the nomination process is stopped temporarily until the public RewardMiningPools method is called by any address. The method distributes tokens from the Mining Governance contract to the top 4 individual Mining Pool contracts. Once this method is complete, the nomination period is open again.

After 48 rewarded Mining Pool smart contracts, the process is reset and the Mining Governance contract will have received more tokens from the Mined Token contract. This process checks the balance of this governance contract and calculates how many rewards to distribute to each of the future 48 Mining Pools that will be funded.

This automated process repeats indefinitely ensuring liquidity mining is always running based on the top liquidity pools by staking weight at the end of each nomination period.

## Constructor

```csharp
public OpdexMiningGovernance(ISmartContractState state,
                             Address minedToken,
                             ulong miningDuration) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | minedToken | The address of the token being mined. |
| `ulong` | miningDuration | The nomination and mining period block duration. |

---

## Properties

| Type | Property | Description |
| `Address` | MinedToken | The address of the token being mined. |
| `ulong` | NominationPeriodEnd | The end block of the current nomination period. |
| `uint` | MiningPoolsFunded | The amount of times mining pools have been funded. |
| `UInt256` | MiningPoolReward | The amount of tokens to reward to a mining pool contract. |
| `bool` | Locked | Contract reentrancy locked status. |
| `bool` | Notified | Flag describing if tokens have been distributed to this contract and that this contract has been notified. Used to allow or deny calculation of the next period's mining rewards per mining pool. |
| `Nomination[]` | Nominations | The top staking pool nominations by weight. |
| `ulong` | MiningDuration | The number of blocks of each nomination/mining period. |

---

## Methods

### Get Mining Pool

Retrieve the mining pool address by the liquidity pool token address.

```csharp
Address GetMiningPool(Address stakingToken);
```

#### Parameters

| Type | Property | Description |
| `Address` | stakingToken | The address of the liquidity pool and its liquidity pool token. |

#### Returns

| Type | Property | Description |
| `Address` | miningPool | The address of the mining pool. |

---

### Nominate Liquidity Pool

Nominate a liquidity pool for liquidity mining based on staking weight. Only the MinedToken contract can make this call.

{: .info }
Only the [MinedToken](#properties) can call this method.

```csharp
void NominateLiquidityPool(Address stakingToken, UInt256 weight);
```

#### Parameters

| Type | Property | Description |
| `Address` | stakingToken | The address of the liquidity pool and its liquidity pool token. |
| `UInt256` | weight | The current balance of staked weight in the liquidity pool. |

---

### Reward Mining Pools

Loops nominations, rewards and notifies mining pool contracts of funding.

{: .info }
The current block number must be greater than the [NominationPeriodEnd](#properties).

```csharp
void RewardMiningPools();
```

---

### Reward Mining Pool

Distributes and notifies the receiving mining pool of rewarded funds.

{: .info }
The current block number must be greater than the [NominationPeriodEnd](#properties).

```csharp
void RewardMiningPool();
```

---

### Notify Distribution

Hook to notify this governance contract that funding from the token has been sent from the mined token contract.

This method is set so only the Mined Token contract can call invoke it. During genesis distribution, the parameters will be used to set the initial first 4 liquidity pools that have mining enabled.

After the genesis distribution, the Mined Token will invoke this method providing `Address.Zero` for the parameter values and they will be ignored.

{: .info }
Only the [MinedToken](#properties) can call this method.

```csharp
void NotifyDistribution(Address firstNomination, Address secondNomination, Address thirdNomination, Address fourthNomination);
```

#### Parameters

| Type | Property | Description |
| `Address` | firstNomination | The first nomination's liquidity pool address. |
| `Address` | secondNomination | The second nomination's liquidity pool address. |
| `Address` | thirdNomination | The third nomination's liquidity pool address. |
| `Address` | fourthNomination | The fourth nomination's liquidity pool address. |

---

## Logs

### Nomination Log

Emitted after a successful nomination of a staking pool.

| Index | Type | Property | Description |
| ✅ | `Address` | StakingPool | The contract address of the staking pool being nominated. |
| ✅ | `Address` | MiningPool | The address of the staking pool's associated mining pool. |
| ❎ | `UInt256` | Weight | The staking weight of the staking pool being nominated. |

---

### Reward Mining Pool Log

Emitted for each mining pool that was successfully nominated and funded.

| Index | Type | Property | Description |
| ✅ | `Address` | StakingPool | The contract address of the liquidity pool token associated with the rewarded mining pool. |
| ✅ | `Address` | MiningPool | The contract address of the mining pool being rewarded tokens to be mined. |
| ❎ | `UInt256` | Amount | The amount of tokens transferred to the mining pool for mining. |

---

## References

#### OpdexMiningGovernance Smart Contract - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Contracts/OpdexMiningGovernance.cs" target="_blank">Github</a>

#### IOpdexMiningGovernance Interface - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Interfaces/IOpdexMiningGovernance.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
