---
layout: page
title: OpdexMarketDeployer
parent: Core Protocol
nav_order: 3
---

<small>Inheritance: [SmartContract](#references), [IOpdexMarketDeployer](#references)</small>

A smart contract that is responsible for deploying new markets. Pre configured staking markets or configurable standard markets can be created by the owner of the market deployer.

A market consists of many liquidity pools that are bound to the market that created them. Markets can be thought of as individual networks of liquidity pools where each market's pools are separate from other markets.

Though markets are separated, public markets and pools can still use flash swaps to arbitrage one another through smart contract integrations.

The deployed staking market is fully public, anybody can create pools, provide liquidity and trade and has a transaction fee of 0.3%.

Standard markets can be configured to enforce address authorizations of pool creators, liquidity providers and/or traders. Standard markets also support a configurable fee ranging from 0% to 1% set at the time of market creation and can enable a market fee which splits all transaction fees between providers and the market owner at 5/6 and 1/6 respectively.

## Constructor

```csharp
public OpdexMarketDeployer(ISmartContractState state) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |

---

## Properties

| Type | Property | Description |
| `Address` | Owner | The address of the owner of the contract. |
| `Address` | PendingOwner | An address that has been promoted by the current contract owner to be the new owner. |

---

## Methods

### Create Standard Market

Creates a configured standard market smart contract. Allows third party public or private markets to be created with one transaction. This method is private and only accessible by the address set as the owner of the market deployer contract.

{: .info }
Only the market deployer owner can call this method.

```csharp
public Address CreateStandardMarket(Address marketOwner, uint transactionFee, bool authPoolCreators, bool authProviders, bool authTraders, bool enableMarketFee);
```

#### Parameters

| Type | Property | Description |
| `Address` | marketOwner | The address to assign as the owner of the market. |
| `uint` | transactionFee | The market transaction fee 0-10 equal to 0-1%. |
| `bool` | authPoolCreators | Flag to authorize liquidity pool creators or not. |
| `bool` | authProviders | Flag to authorize liquidity providers or not. |
| `bool` | authTraders | Flag to authorize traders or not. |
| `bool` | enableMarketFee | Flag to enable the market fee where 1/6 of transaction fees are collected by the market owner. |

#### Returns

| Type | Property | Description |
| `Address` | createdMarket | The contract address of the created market. |

---

### Create Staking Market

Creates a staking market smart contract with a pre-set transaction fee of 0.3%. This method is private and only accessible by the address set as the owner of the market deployer contract.

{: .info }
Only the market deployer owner can call this method.

```csharp
public Address CreateStakingMarket(Address stakingToken);
```

#### Parameters

| Type | Property | Description |
| `Address` | stakingToken | The address to assign as the staking token in the market. |

#### Returns

| Type | Property | Description |
| `Address` | createdMarket | The contract address of the created market. |

---

### Set Pending Ownership

Method to allow the current owner of the market deployer contract to promote a new address as a pending owner.

{: .info }
Only the market deployer owner can call this method.

```csharp
public void SetPendingOwnership(Address pendingOwner);
```

#### Parameters

| Type | Property | Description |
| `Address` | pendingOwner | The address to assign as the new pending owner of the market deployer contract. |

---

### Claim Pending Ownership

Method to allow the pending owner of the market deployer contract to accept ownership.

{: .info }
Only the market deployer pending owner can call this method.

```csharp
public void ClaimPendingOwnership();
```

---

## Logs

### Create Market Log

Emitted when a new market is created.

| Index | Type | Property | Description |
| ✅ | `Address` | Market | The address of the created market smart contract. |
| ✅ | `Address` | Owner | The address set as the owner of the created market. |
| ❎ | `Address` | Router | The address of the created router contract associated with the market. |
| ❎ | `Address` | StakingToken | The market's staking token smart contract address if staking is enabled. For non-staking markets this value is equal to `Address.Zero`. |
| ❎ | `bool` | AuthPoolCreators | Flag describing if pool creators will be authorized or not. |
| ❎ | `bool` | AuthProviders | Flag describing if liquidity providers will be authorized or not. |
| ❎ | `bool` | AuthTraders | Flag describing if traders will be authorized or not. |
| ❎ | `uint` | TransactionFee | The market transaction fee 1-10 equal to 0-1%. |
| ❎ | `bool` | MarketFeeEnabled | Flag describing if the market owner has fees enabled allowing them to collect 1/6 transaction fees. |

---

### Change Deployer Owner Log

Emitted when a the owner of the market deployer contract changes.

| Index | Type | Property | Description |
| ✅ | `Address` | From | The address of the old market deployer owner. |
| ✅ | `Address` | To | The new owner of the market deployer contract. |

---

## References

#### OpdexMarketDeployer Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Deployer/OpdexMarketDeployer.cs" target="_blank">Github</a>

#### IOpdexMarketDeployer Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Deployer/IOpdexMarketDeployer.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
