---
layout: page
title: OpdexLiquidityPool
parent: Core Protocol
nav_order: 1
---

<small>Abstract Class</small>
<small>Inheritance: [SmartContract](#references), [IOpdexLiquidityPool](#references)</small>

An abstract smart contract class that is inherited by [standard pool](opdex-standard-pool) and [staking pool](opdex-staking-pool). Provides a base class sharing similar properties and methods between the two different liquidity pool types.

## Constructor

```csharp
protected OpdexLiquidityPool(ISmartContractState state,
                             Address token,
                             uint transactionFee) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | token | The address of the SRC token in the pool. |
| `uint` | transactionFee | The market transaction fee, 0-10 equal to 0-1%. |

---

## Properties

| Type | Property | Description |
| `string` | Name | The token's name. |
| `string` | Symbol | The token's ticker symbol. |
| `byte` | Decimals | The number of decimal places in the token. |
| `UInt256` | TotalSupply | The total supply of tokens. |
| `Address` | Token | The SRC token in the pool. |
| `ulong` | ReserveCrs | The amount of CRS tokens in reserves. |
| `UInt256` | ReserveSrc | The amount of SRC tokens in reserves. |
| `UInt256` | KLast | The product of the reserves after the previous `Mint` or `Burn` transactions. |
| `bool` | Locked | Contract reentrant lock. |
| `UInt256[]` | Reserves | List of reserve balances. [`AmountCrs`, `AmountSrc`] |
| `ulong` | Balance | Returns the CRS balance of the pool. |
| `uint` | TransactionFee | The pool's transaction fee. |

---

## Methods

### Get Balance

Retrieves the provided addresses token balance.

```csharp
public UInt256 GetBalance(Address address);
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
public UInt256 Allowance(Address owner, Address spender);
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
public bool Approve(Address spender, UInt256 currentAmount, UInt256 amount);
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
public bool TransferFrom(Address from, Address to, UInt256 amount);
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

### Mint

Mints new liquidity pool tokens based on the differences between the contract's actual token balances and reserves. Adding liquidity through a market contract optimistically transfers tokens to this contract before calling this method.

The transfer of CRS and SRC tokens to this contract and the call to this Mint method should be done in the same transaction, through a market contract or an integrated 3rd party contract to avoid front-running, arbitrage, and/or loss of funds.

{: .info }
In private standard markets with provider authorizations enabled, this method will validate the transaction sender is whitelisted for liquidity provisioning.

```csharp
public UInt256 Mint(Address to);
```

#### Parameters

| Type | Property | Description |
| `Address` | to | The address to assign the minted LP tokens to. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountMinted | The number of minted liquidity pool tokens. |

---

### Burn

Burns liquidity pool tokens based on the amount sent optimistically to this contract through a market contract. The contract checks its balance of liquidity pool tokens, burns them and returns the share of reserves to the user.

The transfer of liquidity pool tokens to this contract and the call of this `Burn` method should be done in the same transaction, through a market contract or an integrated 3rd party contract to avoid front-running, arbitrage, and/or loss of funds.

{: .info }
In private standard markets with provider authorizations enabled, this method will validate the transaction sender is whitelisted for liquidity provisioning.

```csharp
public UInt256[] Burn(Address to);
```

#### Parameters

| Type | Property | Description |
| `Address` | to | The address to return the reserves tokens to. |

#### Returns

| Type | Property | Description |
| `UInt256[]` | amountsReceived | Array of CRS and SRC amounts returned. [`AmountCrs`, `AmountSrc`] |

---

### Swap

Swaps tokens in a pool based on the desired amount to pull out of the pool. Through a market contract, the input token is transferred prior to calling this method.

The pool determines which and how many tokens were sent by checking the difference between the pool's token balances and the last recorded reserves. If the amount sent does not satisfy the amount expected to be received, the transaction fails and rolls back.

Each swap transaction incurs a transaction fee set by the market when the pool was created between 0% and 1%. Any amount withdrawn from the pool requires an equal input amount plus transaction fees.

The transfer of CRS or SRC tokens to this contract and the call of this Swap method should be done in the same transaction, through a market contract or an integrated 3rd party contract to avoid front-running, arbitrage, and/or loss of funds.

{: .info }
In private standard markets with trader authorizations enabled, this method will validate the transaction sender is whitelisted for trading.

```csharp
public void Swap(ulong amountCrsOut, UInt256 amountSrcOut, Address to, byte[] data);
```

#### Parameters

| Type | Property | Description |
| `ulong` | amountCrsOut | The amount of CRS tokens to pull from the pool. |
| `UInt256` | amountSrcOut | The amount of SRC tokens to pull from the pool. |
| `Address` | to | The address to send the tokens to. |
| `byte[]` | data | Optional `CallbackData` object bytes for a callback after tokens are pulled from the pool but before validations enforcing the necessary input amount and fees. |

---

### Skim

Forces the balances to equal the current token reserves. In the event of a "donation" or event alike, withdraw the difference between CRS and SRC balance overages and their reserves.

{: .info }
In private standard markets with providers authorizations enabled, this method will validate the transaction sender is whitelisted for liquidity provisioning.

```csharp
public void Skim(Address to);
```

#### Parameters

| Type | Property | Description |
| `Address` | to | The address to send any differences to. |

---

### Sync

Forces the reserves to equal the current token balances. In the event of a "donation" or event alike, update the reserves to reflect the change.

{: .info }
In private standard markets with providers authorizations enabled, this method will validate the transaction sender is whitelisted for liquidity provisioning.

```csharp
public void Sync();
```

---

## Logs

### Transfer Log

Emitted during an SRC20 token transfer transaction.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | From | The address that the tokens were transferred from. |
| ✅ | `Address` | To | The address that the tokens were transferred to. |
| ❎ | `UInt256` | Amount | The amount of tokens that were transferred. |

---

### Approval Log

Emitted during an SRC20 allowance approval transaction.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Owner | The owner's address of the tokens that were approved for spending. |
| ✅ | `Address` | Spender | The spender's address of the tokens that were approved by the owner. |
| ❎ | `UInt256` | OldAmount | The previous allowance amount approved for the spender. |
| ❎ | `UInt256` | Amount | The latest allowance amount approved for the spender. |

---

### Burn Log

Emitted when liquidity pool tokens are burned after removing liquidity from a pool.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Sender | The sender of the contract call that burned tokens. |
| ✅ | `Address` | To | The address that liquidated pool reserves were transferred to. |
| ❎ | `ulong` | AmountCrs | The amount of CRS tokens that were pulled from the pool. |
| ❎ | `UInt256` | AmountSrc | The amount of SRC20 tokens that were pulled from the pool. |
| ❎ | `UInt256` | AmountLpt | The amount of liquidity pool tokens that were burned. |

---

### Mint Log

Emitted when liquidity pool tokens are minted after adding liquidity to a pool.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Sender | The sender of the contract call that minted tokens. |
| ✅ | `Address` | To | The address to assign the newly minted liquidity pool tokens to. |
| ❎ | `ulong` | AmountCrs | The amount of CRS tokens that were deposited to the pool. |
| ❎ | `UInt256` | AmountSrc | The amount of SRC20 tokens that were deposited to the pool. |
| ❎ | `UInt256` | AmountLpt | The amount of liquidity pool tokens that were minted. |

---

### Reserves Log

Emitted when a pool's reserves are updated, after a swap, adding, or removing liquidity transaction.

#### Properties

| Index | Type | Property | Description |
| ❎ | `ulong` | ReserveCrs | The amount of CRS tokens in the pool's reserves. |
| ❎ | `UInt256` | ReserveSrc | The amount of SRC20 tokens in the pool's reserves. |

---

### Swap Log

Emitted for each pool in the transaction where a token swap occurred.

#### Properties

| Index | Type | Property | Description |
| ✅ | `Address` | Sender | The sender of the contract call that swapped tokens. |
| ✅ | `Address` | To | The address to send received tokens to. |
| ❎ | `ulong` | AmountCrsIn | The amount of CRS tokens deposited to the pool for a swap. |
| ❎ | `UInt256` | AmountSrcIn | The amount of SRC20 tokens deposited to the pool for a swap. |
| ❎ | `ulong` | AmountCrsOut | The amount of CRS tokens withdrawn from the pool. |
| ❎ | `UInt256` | AmountSrcOut | The amount of SRC20 tokens withdrawn from the pool. |

---

## References

#### OpdexLiquidityPool Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Pools/OpdexLiquidityPool.cs" target="_blank">Github</a>

#### IOpdexLiquidityPool Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Pools/IOpdexLiquidityPool.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
