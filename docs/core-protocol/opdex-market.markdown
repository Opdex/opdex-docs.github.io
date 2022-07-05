---
layout: page
title: OpdexMarket
parent: Core Protocol
nav_order: 2
---

<small>Abstract Class</small>
<small>Inheritance: [SmartContract](#references), [IOpdexMarket](#references)</small>

An abstract smart contract class that is inherited by [standard market](opdex-standard-market) and [staking market](opdex-staking-market). Provides a base class sharing similar properties and methods between the two different market types.

## Constructor

```csharp
protected OpdexMarket(ISmartContractState state, uint transactionFee) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |

---

## Properties

| Type | Property | Description |
| `uint` | TransactionFee | The staking market transaction fee set at 3 equal to 0.3%. |

---

## Methods

### Get Pool

Retrieve a pool's contract address by the SRC20 token associated.

```csharp
public Address GetPool(Address token);
```

#### Parameters

| Type | Property | Description |
| `Address` | token | The address of the SRC token. |

#### Returns

| Type | Property | Description |
| `Address` | pool | The address of the requested pool. |

---

### Create Pool

Creates a new liquidity pool for the provided token with 0 liquidity if it doesn't already exist.

{: .info }
Some standard markets may have authorization implemented on this method requiring address whitelisting.

```csharp
public Address CreatePool(Address token);
```

#### Parameters

| Type | Property | Description |
| `Address` | token | The address of the SRC token. |

#### Returns

| Type | Property | Description |
| `Address` | pool | The address of the created pool. |

---

## Logs

### Create Liquidity Pool Log

Emitted when a new liquidity pool is created.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Token | The address of the SRC token in the liquidity pool. |
| ✅ | `Address` | Pool | The address of the new liquidity pool smart contract. |

---

## References

#### OpdexMarket Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Markets/OpdexMarket.cs" target="_blank">Github</a>

#### IOpdexMarket Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Markets/IOpdexMarket.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
