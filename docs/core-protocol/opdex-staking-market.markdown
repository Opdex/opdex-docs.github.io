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

## Methods

### Is Authorized

Checks the if the provided address is authorized for the given permission.

```csharp
public bool IsAuthorized(Address address, byte permission);
```

#### Parameters

| Type | Property | Description |
| `Address` | address | The address to check permissions for. |
| `byte` | permission | The permission to check authorization of. (1 - Create Pool, 2 - Trade, 3 - Provide, 4 - Set Permissions) |

#### Returns

| Type | Property | Description |
| `bool` | isAuthorized | Flag describing if the address is authorized or not. |

---

### Is Authorized - Multi

Checks the if the provided addresses are authorized for the given permission.

```csharp
public bool IsAuthorized(Address primary, Address secondary, byte permission);
```

#### Parameters

| Type | Property | Description |
| `Address` | primary | The primary address to check permissions for. |
| `Address` | secondary | The secondary address to check permissions for. |
| `byte` | permission | The permission to check authorization of. (1 - Create Pool, 2 - Trade, 3 - Provide, 4 - Set Permissions) |

#### Returns

| Type | Property | Description |
| `bool` | isAuthorized | Flag describing if both of the addresses are authorized or not. |

---

### Authorize

Allows permitted addresses to set an authorization for a provided address and permission.

{: .info }
Only the market owner or addresses authorized to set permissions can call this method.

```csharp
public void Authorize(Address address, byte permission, bool authorize);
```

#### Parameters

| Type | Property | Description |
| `Address` | address | The address to set the permission for. |
| `byte` | permission | The permission being set. See Permissions for list of available options. |
| `bool` | authorize | Flag describing if the address should be authorized or not. |

---

### Set Pending Ownership

Public method allowing the current contract owner to whitelist a new pending owner. The newly pending owner Claim Pending Ownership to accept ownership.

{: .info }
Only the address set as the market owner can call this method.

```csharp
public void SetPendingOwnership(Address pendingOwner);
```

#### Parameters

| Type | Property | Description |
| `Address` | pendingOwner | The address to set as the new pending owner. |

---

### Claim Pending Ownership

Public method to allow the pending new owner to accept ownership replacing the current contract owner.

{: .info }
Only the address set as the pending owner can call this method.

```csharp
void ClaimPendingOwnership();
```

---

### Collect Market Fees

Looks up a pool by the provided SRC token address and collects the specified amount of market fees (LP tokens). Collected fees from the pool are transferred to the current owner of the market.

See [IOpdexLiquidityPool.GetBalance](opdex-liquidity-pool#get-balance) for retrieving the current amount of fees available for collection held by the market contract.

{: .info }
Only the address set as the market owner can call this method.

```csharp
public void CollectMarketFees(Address token, UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `Address` | token | The SRC token address to lookup the liquidity pool by. |
| `UInt256` | amount | The amount of fees (LP tokens) to collect from the pool and transfer to the market owner. |

---

## Logs

### Change Market Permission Log

Emitted when an addresses permissions change within a market.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Address | The address affected by the permission change. |
| ❎ | `byte` | Permission | The permission being modified. |
| ❎ | `bool` | IsAuthorized | Flag determining if the address is authorized for the permission. |

---

### Set Pending Ownership Log

Emitted when a new address is set as the pending market owner.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | From | The current market owner's wallet address. |
| ✅ | `Address` | To | The pending market owner's wallet address. |

---

### Claim Pending Ownership Log

Emitted when a pending address accepts ownership of the market.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | From | The current market owner's wallet address. |
| ✅ | `Address` | To | The new market owner's wallet address. |

---

## References

#### OpdexMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexMarket.cs" target="_blank">Github</a>

#### OpdexStakingMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexStakingMarket.cs" target="_blank">Github</a>

#### IOpdexStakingMarket Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Markets/IOpdexStakingMarket.cs" target="_blank">Github</a>
