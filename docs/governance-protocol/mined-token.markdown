---
layout: page
title: Mined Token
parent: Governance Protocol
nav_order: 1
---

<small>Inheritance: [SmartContract](#references), [IOpdexMinedToken](#references)</small>

A standard SRC20 token contract with added mining and governance abilities. Distributes tokens based on a schedule to two smart contracts, a mining governance and a vault smart contract that are both created when deploying this token contract.

A schedule set when creating the token contract determines the number of the tokens distributed each period to the mining governance and vault smart contracts. The mining governance distributes tokens to nominated mining pools and the vault locks tokens using vault certificates.

The token is obtained through liquidity mining using nominated liquidity pool tokens. Once mined, the token is used to participate in governance by staking in liquidity pools.

By staking tokens, the protocol automatically notifies this token contract to report the liquidity pool's staking weight to the mining governance contract as a liquidity pool nomination.

At the end of each nomination period the top 4 liquidity pools by staking weight will have liquidity mining enabled.

In summary, the mined token is obtained through liquidity mining with liquidity pool tokens. Once tokens have been mined and collected, they are intended to be staked in liquidity pools to nominate the pool for the next round of liquidity mining.

## Constructor

```csharp
public OpdexMinedToken(ISmartContractState state,
                       string name,
                       string symbol,
                       byte[] vaultDistribution,
                       byte[] miningDistribution,
                       ulong periodDuration,
                       ulong vaultTotalPledgeMinimum,
                       ulong vaultTotalVoteMinimum) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `string` | name | The name of the token. |
| `string` | symbol | The token ticker symbol. |
| `byte[]` | vaultDistribution | Serialized UInt256 array of vault distribution amounts. |
| `byte[]` | miningDistribution | Serialized UInt256 array of mining distribution amounts. |
| `ulong` | periodDuration | The number of blocks between token distributions. |
| `ulong` | vaultTotalPledgeMinimum | The minimum total number of tokens pledged to a proposal to move to a vote. |
| `ulong` | vaultTotalVoteMinimum | The minimum total number of tokens voted on a proposal to have a chance to be approved. |

---

## Properties

| Type | Property | Description |
| `string` | Name | The name of the token. |
| `string` | Symbol | The token ticker symbol. |
| `byte` | Decimals | The number of decimal places in the token. |
| `UInt256` | TotalSupply | The total supply of tokens. |
| `Address` | Creator | The address of the creator of the token contract. |
| `Address` | MiningGovernance | The address of the mining governance contract. |
| `Address` | Vault | The address of the vault contract. |
| `UInt256[]` | MiningSchedule | The scheduled amounts to mint for mining. |
| `UInt256[]` | VaultSchedule | The scheduled amounts to mint to the vault. |
| `ulong` | Genesis | The initial block the token can start being distributed at. |
| `uint` | PeriodIndex | The number of periods that have been distributed. |
| `ulong` | PeriodDuration | The number of blocks between token distribution periods. |

---

## Methods

### Get Balance

Retrieves the provided addresses token balances.

```csharp
UInt256 GetBalance(Address address);
```

#### Parameters

| Type | Property | Description |
| `Address` | address | The address to check the balance for. |

#### Returns

| Type | Property | Description |
| `UInt256` | balance | The balance of the provided address. |

---

### Allowance

Retrieves the allowance of a spender approved by an owner.

```csharp
UInt256 Allowance(Address owner, Address spender);
```

#### Parameters

| Type | Property | Description |
| `Address` | owner | The address of the owner of the tokens. |
| `Address` | spender | The address of the spender of the tokens. |

#### Returns

| Type | Property | Description |
| `UInt256` | allowance | The spender's allowance of the owner's tokens. |

---

### Approve

Approves an allowance for a spender.

```csharp
UInt256 Approve(Address spender, UInt256 currentAmount, UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `Address` | spender | The address to be the spender of the tokens. |
| `UInt256` | currentAmount | The spender's current approved allowance. |
| `UInt256` | amount | The updated allowance of the spender. |

#### Returns

| Type | Property | Description |
| `bool` | success | Flag describing if the transaction was successful or not. |

---

### Transfer From

Allows the sender to transfer tokens using an approved allowance from another address.

```csharp
bool TransferFrom(Address from, Address to, UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `Address` | from | The address to spend an approved allowance from. |
| `Address` | to | The address to transfer tokens to. |
| `UInt256` | amount | The amount of tokens to transfer. |

#### Returns

| Type | Property | Description |
| `bool` | success | Flag describing if the transaction was successful or not. |

---

### Distribute

Mints and distributes tokens according to the vault and mining period schedules used after genesis distribution.

```csharp
void Distribute();
```

---

### Distribute Genesis

Mints and distributions genesis tokens while also nominating the first four liquidity pools to have liquidity mining enabled.

```csharp
void DistributeGenesis(Address firstNomination,
                       Address secondNomination,
                       Address thirdNomination,
                       Address fourthNomination);
```

#### Parameters

| Type | Property | Description |
| `Address` | firstNomination | The first nomination's liquidity pool address. |
| `Address` | secondNomination | The second nomination's liquidity pool address. |
| `Address` | thirdNomination | The third nomination's liquidity pool address. |
| `Address` | fourthNomination | The fourth nomination's liquidity pool address. |

---

### Nominate Liquidity Pool

Nominates a liquidity pool by its staking weight for liquidity mining. The caller must be a smart contract and must have a staking token balance.

```csharp
void NominateLiquidityPool();
```

---

## Logs

### Approval Log

Emitted during an SRC20 allowance approval transaction.

| Index | Type | Property | Description |
| ✅ | `Address` | Owner | The owner's address of the tokens that were approved for spending. |
| ✅ | `Address` | Spender | The spender's address of the tokens that were approved by the owner. |
| ❎ | `UInt256` | OldAmount | The previous allowance amount approved for the spender. |
| ❎ | `UInt256` | Amount | The latest allowance amount approved for the spender. |

---

### Transfer Log

Emitted during an SRC20 token transfer transaction.

| Index | Type | Property | Description |
| ✅ | `Address` | From | The address that the tokens were transferred from. |
| ✅ | `Address` | To | The address that the tokens were transferred to. |
| ❎ | `UInt256` | Amount | The amount of tokens that were transferred. |

---

### Distribution Log

Emitted when tokens are distributed to the vault and mining governance contracts.

| Index | Type | Property | Description |
| ❎ | `UInt256` | VaultAmount | The amount of tokens distributed to the vault. |
| ❎ | `UInt256` | MiningAmount | The amount of tokens distributed to the mining governance contract. |
| ✅ | `uint` | PeriodIndex | The current distribution period number, incremented after each successful distribution. |

---

## References

#### OpdexMinedToken Smart Contract - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Contracts/OpdexMinedToken.cs" target="_blank">Github</a>

#### IOpdexMinedToken Interface - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Interfaces/IOpdexMinedToken.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
