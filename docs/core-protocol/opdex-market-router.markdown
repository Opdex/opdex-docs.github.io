---
layout: page
title: OpdexMarketRouter
parent: Core Protocol
nav_order: 4
---

<small>Inheritance: [SmartContract](#references), [IOpdexRouter](#references)</small>

A router contract acts as the primary entry point for exchange based, multi-step transactions within a single market. This contract is responsible for routing quotes, swaps, adding or removing liquidity based transactions.

Transactions are quoted, prerequisite allowance transfers are made and hops between pools are possible during a single transaction. This allows for optimistic transfers and ultimately, savings in gas costs.

Because these transaction types require multiple steps and optimistic transfers, users should not call liquidity pool smart contracts directly. The above mentioned transaction types should be called through this contract only or a trusted third party smart contract integrated with Opdex protocols.

Third party smart contracts can be written to perform these multi-step transactions for integrations directly with pools. These types of integrations can take advantage of flash swaps (borrowing), arbitrage opportunities and other use cases.

## Constructor

```csharp
public OpdexRouter(ISmartContractState state,
                   Address market,
                   uint transactionFee,
                   bool authProviders,
                   bool authTraders) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | market | The address of the market associated. |
| `uint` | transactionFee | 0-10 transaction fee equal to 0-1%. |
| `bool` | authProviders | Flag indicating if liquidity providers should be authorized. |
| `bool` | authTraders | Flag indicating if traders should be authorized. |

---

## Properties

| Type | Property | Description |
| `Address` | Market | The address of the market the router is intended for. |
| `uint` | TransactionFee | The market transaction fee 0-10 equal to 0-1%. |
| `bool` | AuthProviders | Flag that the router uses to enforce liquidity provider authorizations according to the rules of the associated market. |
| `bool` | AuthTraders | Flag that the router uses to enforce trader authorizations according to the rules of the associated market. |

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

### Get Liquidity Quote

Calculates the value of amountB to be deposited with amountA to a pool based on the pool's current reserves.

```csharp
public UInt256 GetLiquidityQuote(UInt256 amountA, UInt256 reserveA, UInt256 reserveB);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountA | The amount of TokenA tokens to provide. |
| `UInt256` | reserveA | The pool's reserve amount of TokenA. |
| `UInt256` | reserveB | The pool's reserve of the TokenB. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountB | The amount of necessary tokens to provide for TokenB. |

---

### Add Liquidity

Allows users to provide liquidity to a pool, provided amounts must match the same ratio as the pool's current reserves.

SRC20 tokens used for providing liquidity must have previously approved an allowance for this contract to spend.

See [Get Liquidity Quote](#get-liquidity-quote) for quoting how many tokens are needed to match the liquidity pool's reserve ratio.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthProviders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256[] AddLiquidity(Address token, UInt256 amountSrcDesired, ulong amountCrsMin, UInt256 amountSrcMin, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `Address` | token | The SRC token address to lookup its pool by. |
| `UInt256` | amountSrcDesired | The wishful amount of SRC tokens to deposit. |
| `ulong` | amountCrsMin | The minimum amount of CRS tokens to deposit. |
| `UInt256` | amountSrcMin | The minimum amount of SRC tokens to deposit. |
| `Address` | to | The address to deposit the liquidity pool tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256[]` | amounts | The final amounts used to provide liquidity. [`amountCrsIn`, `amountSrcIn`, `amountLptOut`] |

---

### Remove Liquidity

Allows users to remove liquidity from a pool. Burns the pool's liquidity pool tokens in return, sending the user's share of the pool's reserves to the specified address.

Remove liquidity from a specified pool. Liquidity Pool tokens being removed and burned must have previously approved the controller contract with the desired burn amount.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthProviders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256[] RemoveLiquidity(Address token, UInt256 liquidity, ulong amountCrsMin, UInt256 amountSrcMin, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `Address` | token | The SRC token address to lookup its pool by. |
| `UInt256` | liquidity | The amount of liquidity pool tokens to remove. |
| `ulong` | amountCrsMin | The minimum amount of CRS tokens acceptable to receive. |
| `UInt256` | amountSrcMin | The minimum amount of SRC tokens acceptable to receive. |
| `Address` | to | The address to send the CRS and SRC tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256[]` | amounts | The amounts of received reserve tokens. [`amountCrsOut`, `amountSrcOut`] |

---

### Swap Exact CRS for SRC

Swaps an exact amount of CRS tokens for a set minimum amount of SRC tokens.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256 SwapExactCrsForSrc(UInt256 amountSrcOutMin, Address token, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountSrcOutMin | The minimum amount of SRC tokens acceptable to receive. |
| `Address` | token | The SRC token address to lookup its pool by. |
| `Address` | to | The address to send the received SRC tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountSrcOut | The final amount of SRC tokens received. |

---

### Swap Exact SRC for CRS

Swaps an exact amount of SRC tokes for a minimum amount of CRS tokens. Swapped SRC tokens must have previously approved the controller contract with the desired amount.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public ulong SwapExactSrcForCrs(UInt256 amountSrcIn, ulong amountCrsOutMin, Address token, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountSrcIn | The exact amount of SRC tokens to swap. |
| `ulong` | amountCrsOutMin | The minimum amount of CRS tokens to receive. |
| `Address` | token | The SRC token address to lookup its pool by. |
| `Address` | to | The address to send the CRS tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountCrsOut | The final amount of CRS tokens received. |

---

### Swap Exact SRC for SRC

Swaps an exact amount of SRC tokens for a minimum amount of SRC tokens. SRC tokens being swapped must have previously approved the controller contract with the desired amount.

SRC to SRC token swaps hop between two pools which incurs double the market's transaction fees but is done in a single transaction. Input SRC tokens are swapped for CRS tokens which are then swapped for the desired SRC token.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256 SwapExactSrcForSrc(UInt256 amountSrcIn, Address tokenIn, UInt256 amountSrcOutMin, Address tokenOut, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountSrcIn | The amount of SRC tokens to swap. |
| `Address` | tokenIn | The address of the SRC token being swapped. |
| `UInt256` | amountSrcOutMin | The minimum amount of SRC tokens to receive. |
| `Address` | tokenOut | The address of the SRC token being received. |
| `Address` | to | The address to send the SRC tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountSrcOut | The final amount of SRC tokens received. |

---

### Swap CRS for Exact SRC

Swaps a maximum amount of CRS tokens for an exact amount of SRC tokens. SRC tokens must have previously approved the controller contract with the desired amount.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public ulong SwapCrsForExactSrc(UInt256 amountSrcOut, Address token, Address to, ulong deadline)
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountSrcOut | The exact amount of SRC tokens to receive. |
| `Address` | token | The SRC token address to lookup its pool by. |
| `Address` | to | The address to send the SRC tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountCrsIn | The final amount of CRS tokens swapped. |

---

### Swap SRC for Exact CRS

Swaps a maximum amount of SRC tokens for an exact amount of CRS tokens. SRC tokens must have previously approved the controller contract with the desired amount.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256 SwapSrcForExactCrs(ulong amountCrsOut, UInt256 amountSrcInMax, Address token, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `ulong` | amountCrsOut | The exact amount of CRS tokens to receive. |
| `UInt256` | amountSrcInMax | The maximum amount of SRC tokens to swap. |
| `Address` | token | The SRC token address to lookup its pool by. |
| `Address` | to | The address to send the CRS tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountSrcIn | The final amount of SRC tokens swapped. |

---

### Swap SRC for Exact SRC

Swaps a maximum amount of SRC tokens for an exact amount of SRC tokens. SRC tokens being swapped must have previously approved the controller contract with the desired amount.

SRC to SRC token swaps hop between two pools which incurs double the market's transaction fees but is done in a single transaction. Input SRC tokens are swapped for CRS tokens which are then swapped for the desired SRC token.

{: .info }
A router contract is tied to a specific market. If that market is a [standard market](opdex-standard-market) and has `AuthTraders` enabled, only whitelisted addresses may call this method.

```csharp
public UInt256 SwapSrcForExactSrc(UInt256 amountSrcInMax, Address tokenIn, UInt256 amountSrcOut, Address tokenOut, Address to, ulong deadline);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountSrcInMax | The maximum amount of SRC tokens to swap. |
| `Address` | tokenIn | The address of the SRC token being swapped. |
| `UInt256` | amountSrcOut | The exact amount of SRC tokens to receive. |
| `Address` | tokenOut | The address of the SRC token being received. |
| `Address` | to | The address to send the SRC tokens to. |
| `ulong` | deadline | Block number deadline to execute the transaction by. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountSrcIn | The final amount of SRC tokens swapped. |

---

### Get Amount In (CRS-SRC)

Calculates the necessary deposit amount, including transaction fees, based on the amount to receive and the pool's reserves. Used for CRS-SRC or SRC-CRS single pool transactions.

```csharp
public UInt256 GetAmountIn(UInt256 amountOut, UInt256 reserveIn, UInt256 reserveOut);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountOut | The amount of tokens to receive. |
| `UInt256` | reserveIn | The pool's reserve amount of the input token type. |
| `UInt256` | reserveOut | The pool's reserve amount of the output token type. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountIn | The number of tokens to deposit. |

---

### Get Amount In (SRC-SRC)

Calculates the necessary SRC deposit amount, including transaction fees, based on the amount to receive and the pool's reserves. Used for SRC-SRC multi-pool transactions.

```csharp
public UInt256 GetAmountIn(UInt256 tokenOutAmount, UInt256 tokenOutReserveCrs, UInt256 tokenOutReserveSrc, UInt256 tokenInReserveCrs, UInt256 tokenInReserveSrc);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | tokenOutAmount | The amount of SRC tokens to receive. |
| `UInt256` | tokenOutReserveCrs | The pool's CRS reserve amount of the output token type. |
| `UInt256` | tokenOutReserveSrc | The pool's SRC reserve amount of the output token type. |
| `UInt256` | tokenInReserveCrs | The pool's CRS reserve amount of the input token type. |
| `UInt256` | tokenInReserveSrc | The pool's SRC reserve amount of the input token type. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountIn | The number of SRC tokens to deposit. |

---

### Get Amount Out (CRS-SRC)

Calculate the amount returned after transaction fees based on the token input amount and the pool's reserves. Used for CRS-SRC or SRC-CRS single pool transactions.

```csharp
UInt256 GetAmountOut(UInt256 amountIn, UInt256 reserveIn, UInt256 reserveOut);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amountIn | The amount of the token to deposit. |
| `UInt256` | reserveIn | The pool's reserve amount of the input token type. |
| `UInt256` | reserveOut | The pool's reserve amount of the output token type. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountOut | The number of tokens to receive. |

---

### Get Amount Out (SRC-SRC)

Calculates the amount of SRC tokens returned after transaction fees based on the token input amount and the pool's reserves. Used for SRC-SRC multi-pool transactions.

```csharp
public UInt256 GetAmountOut(UInt256 tokenInAmount, UInt256 tokenInReserveCrs, UInt256 tokenInReserveSrc, UInt256 tokenOutReserveCrs, UInt256 tokenOutReserveSrc);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | tokenInAmount | The amount of SRC tokens necessary to deposit. |
| `UInt256` | tokenInReserveCrs | The pool's CRS reserve amount of the input token type. |
| `UInt256` | tokenInReserveSrc | The pool's SRC reserve amount of the input token type. |
| `UInt256` | tokenOutReserveCrs | The pool's CRS reserve amount of the output token type. |
| `UInt256` | tokenOutReserveSrc | The pool's SRC reserve amount of the output token type. |

#### Returns

| Type | Property | Description |
| `UInt256` | amountOut | The number of SRC tokens to receive. |

---

## References

#### OpdexRouter Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Routers/OpdexRouter.cs" target="_blank">Github</a>

#### IOpdexRouter Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Routers/IOpdexRouter.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
