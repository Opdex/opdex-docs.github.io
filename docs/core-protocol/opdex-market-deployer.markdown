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

## References

#### OpdexMarketDeployer Smart Contract - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Contracts/Deployer/OpdexMarketDeployer.cs" target="_blank">Github</a>

#### IOpdexMarketDeployer Interface - <a href="https://github.com/Opdex/opdex-v1-core/blob/main/src/Interfaces/Deployer/IOpdexMarketDeployer.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
