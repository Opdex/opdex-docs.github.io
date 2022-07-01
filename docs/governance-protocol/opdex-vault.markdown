---
layout: page
title: OpdexVault
parent: Governance Protocol
nav_order: 3
---

<small>Inheritance: [SmartContract](#references), [IOpdexVault](#references)</small>

A smart contract used to lock and distribute tokens based on created certificates through community approved proposals.

The mined token contract distributes the allotted amount yearly to the mining governance and vault contracts. The vault contract will automatically receive tokens and update total supply of locked and unassigned vault tokens.

Proposals can be created to be voted on and when approved, assign tokens to a certificate within the vault, to vest then redeem after about a year (1,971,000 blocks). Proposals can also be created for revoking vault certificates during vesting, changing the total pledge minimum amount and changing the total vote minimum amount.

Proposal creation is public, meaning anyone can do it, however, proposals will not be put into affect unless they pass the minimum pledge amount, the minimum vote amount, and there must be more votes in favor than in opposition.

## Constructor

```csharp
public OpdexVault(ISmartContractState state,
                  Address token,
                  ulong vestingDuration,
                  ulong totalPledgeMinimum,
                  ulong totalVoteMinimum,
                  ulong vestingDuration) : base(state)
```

#### Parameters

| Type | Property | Description |
| `ISmartContractState` | state | Dependency injected smart contract state. <span style="color: #f7556b;"> Omitted from parameters during deployment.</span> |
| `Address` | token | The locked SRC token. |
| `ulong` | totalPledgeMinimum | The minimum total number of tokens pledged to a proposal to move to a vote. |
| `ulong` | totalVoteMinimum | The minimum total number of tokens voted on a proposal to have a chance to be approved. |
| `ulong` | vestingDuration | The length in blocks of the vesting period. |

---

## Properties

| Type | Property | Description |
| `Address` | Token | The SRC20 token the vault is responsible for locking. |
| `UInt256` | TotalSupply | The total supply of tokens available to be assigned to certificates. |
| `ulong` | VestingDuration | The number of blocks required for certificates to be locked before being redeemed. |
| `ulong` | NextProposalId | The Id of the next proposal to be created. |
| `UInt256` | TotalProposedAmount | The total number of tokens requested for active create certificate proposals. |
| `ulong` | TotalPledgeMinimum | The minimum number of CRS tokens required to pledge to move a proposal on to a vote. |
| `ulong` | TotalVoteMinimum | The minimum number of CRS tokens required to vote on a proposal to have a chance to pass. |

---

## Methods

### Get Certificate

Retrieves a certificate a given address is assigned.

```csharp
Certificate GetCertificate(Address wallet);
```

#### Parameters

| Type | Property | Description |
| `Address` | wallet | The wallet address to check for a certificate. |

#### Returns

| Type | Property | Description |
| `Certificate` | certificate | A certificate issued to the provided address. |

#### Certificate

| Type | Property | Description |
| `UInt256` | Amount | The amount of tokens the certificate has assigned. |
| `ulong` | VestedBlock | The block number that the certificate is fully vested at. |
| `bool` | Revoked | Whether or not the certificate has been revoked. |

---

### Get Proposal

Retrieves the details about a proposal.

```csharp
ProposalDetails GetProposal(ulong proposalId);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The ID number of the requested proposal. |

#### Returns

| Type | Property | Description |
| `ProposalDetails` | proposalDetails | Properties used to measure a proposals progress and proposition. |

#### Proposal Details

| Type | Property | Description |
| `UInt256` | Amount | The amount of the proposal. For create or revoke certificates, will equal the amount of ODX tokens assigned. For pledge or vote minimum amount proposals, is equal to the proposed new amount. |
| `Address` | Wallet | The address of the primary recipient or wallet of a proposal. For revoke certificate proposals, is equal to the certificate's holder. For pledge and minimum vote proposals, is equal to the creator of the proposal. For new certificates, is equal to the wallet address that will have a certificate assigned. |
| `byte` | Type | The type of proposal. |
| `byte` | Status | The status of the proposal. |
| `ulong` | Expiration | The expiration block of the current status. |
| `ulong` | YesAmount | The number of CRS voted Yes on a proposal during the vote stage. |
| `ulong` | NoAmount | The number of CRS voted No on a proposal during the vote stage. |
| `ulong` | PledgeAmount | The number of CRS pledged to the proposal during the pledge stage. |

---

### Get Proposal Vote

Retrieve the vote details of a voter for a specific proposal.

```csharp
ProposalVote GetProposalVote(ulong proposalId, Address voter);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The ID number of the proposal. |
| `Address` | voter | The voter's address. |

#### Returns

| Type | Property | Description |
| `ProposalVote` | proposalVote | Proposal vote details of the how an address voted. |

#### Proposal Vote

| Type | Property | Description |
| `bool` | InFavor | True if the vote was in favor of the proposal, otherwise false. |
| `ulong` | Amount | The amount of CRS tokens actively locked toward the voter's vote. |

---

### Get Proposal Pledge

Retrieve the amount of tokens pledged by an address for a proposal.

```csharp
ulong GetProposalPledge(ulong proposalId, Address pledger);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The ID of the proposal to retrieve the pledge for. |
| `Address` | pledger | The address of the pledger. |

#### Returns

| Type | Property | Description |
| `ulong` | pledge | The number of CRS tokens actively pledged to the proposal by the provided wallet address. |

---

### Get Certificate Proposal Id By Recipient

Retrieves an Id of a certificate proposal that the specified recipient has open, limiting to one proposal per recipient for certificate based proposals at a time.

```csharp
ulong GetCertificateProposalIdByRecipient(Address recipient);
```

#### Parameters

| Type | Property | Description |
| `Address` | recipient | The address of the certificate recipient. |

#### Returns

| Type | Property | Description |
| `ulong` | pledge | The proposal id of an open proposal for the provided recipient. |

---

### Notify Distribution

Method to allow the vault token to notify the vault of distribution to update the total supply.

```csharp
void NotifyDistribution(UInt256 amount);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of tokens sent to the vault. |

---

### Redeem Certificate

Redeems a vested certificate the sender owns and transfers the claimed tokens.

```csharp
void RedeemCertificate();
```

---

### Create New Certificate Proposal

Creates a new proposal for a certificate to be created. If this proposal were to pass, the recipient would receive a certificate that is vested for the VestingDuration, entitling them to an amount of governance tokens in the vault.

```csharp
ulong CreateNewCertificateProposal(UInt256 amount, Address recipient, string description);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The amount of tokens proposed the recipient's certificate be assigned. |
| `Address` | recipient | The recipient address to assign a certificate to if approved. |
| `string` | description | A description of the proposal, limited to 200 characters, preferably a link. |

#### Returns

| Type | Property | Description |
| `ulong` | proposalId | The ID of the created proposal. |

---

### Create Revoke Certificate Proposal

Creates a new proposal for a certificate to be revoked. If this proposal were to pass, the certificate holder would only be entitled to the governance tokens accrued in the VestingDuration, up to the block at which the certificate is revoked.

```csharp
ulong CreateRevokeCertificateProposal(Address recipient, string description);
```

#### Parameters

| Type | Property | Description |
| `Address` | recipient | The recipient of an existing certificate to have revoked. |
| `string` | description | A description of the proposal, limited to 200 characters, preferably a link. |

#### Returns

| Type | Property | Description |
| `ulong` | proposalId | The ID of the created proposal. |

---

### Create Total Pledge Minimum Proposal

Creates a new proposal for a change to the minimum pledge total amount. If this proposal were to pass, it would alter the total amount of tokens required to pledge to a proposal, so that it can move on to a vote.

```csharp
ulong CreateTotalPledgeMinimumProposal(UInt256 amount, string description);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The proposed new total pledge minimum amount. |
| `string` | description | A description of the proposal, limited to 200 characters, preferably a link. |

#### Returns

| Type | Property | Description |
| `ulong` | proposalId | The ID of the created proposal. |

---

### Create Total Vote Minimum Proposal

Creates a new proposal for a change to the minimum vote total amount. If this proposal were to pass, it would alter the total amount of tokens required to vote on a proposal, so that it can have a chance to pass.

```csharp
ulong CreateTotalVoteMinimumProposal(UInt256 amount, string description);
```

#### Parameters

| Type | Property | Description |
| `UInt256` | amount | The proposed new total vote minimum amount. |
| `string` | description | A description of the proposal, limited to 200 characters, preferably a link. |

#### Returns

| Type | Property | Description |
| `ulong` | proposalId | The ID of the created proposal. |

---

### Pledge

Pledge for a proposal by temporarily locking CRS tokens to help meet the minimum pledge amount, so that it can move to an official vote.

```csharp
void Pledge(ulong proposalId);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The Id of the proposal to pledge to. |

---

### Vote

Votes for or against a proposal by temporarily holding CRS in contract as vote weight.

```csharp
void Vote(ulong proposalId, bool inFavor);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The Id number of the proposal being voted on. |
| `bool` | inFavor | True or false indicating if the vote is in favor of the proposal or against it. |

---

### Withdraw Pledge

Withdraws CRS tokens from a proposal pledge, removing the pledge if still in progress.

```csharp
void WithdrawPledge(ulong proposalId, ulong withdrawAmount);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The Id of the proposal to remove a pledge amount from. |
| `ulong` | withdrawAmount | The amount to withdraw from a pledge. |

---

### Withdraw Vote

Withdraws held CRS from proposal vote, removing the vote if still in progress.

```csharp
void WithdrawVote(ulong proposalId, ulong withdrawAmount);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The Id number of the proposal to vote on. |
| `ulong` | withdrawAmount | The amount of tokens to withdraw from the proposal vote. |

---

### Complete Proposal

Completes a proposal and executes the resulting command within the vault contract if approved.

```csharp
void CompleteProposal(ulong proposalId);
```

#### Parameters

| Type | Property | Description |
| `ulong` | proposalId | The Id number of the proposal to complete. |

---

## Logs

### Create Vault Certificate Log

Emitted when a vault certificate is created, either for the vault owner during each distribution period or from the owner to a new certificate holder.

| Index | Type | Property | Description |
| ✅ | `Address` | Owner | The address of the holder of the created certificate. |
| ❎ | `UInt256` | Amount | The amount of tokens locked in the certificate. |
| ❎ | `ulong` | VestedBlock | The block number that the certificate will be eligible for redemption at. |

---

### Redeem Vault Certificate Log

Emitted when an address redeems their certificate and claims their tokens.

| Index | Type | Property | Description |
| ✅ | `Address` | Owner | The address of the holder of the redeemed certificate. |
| ❎ | `UInt256` | Amount | The amount of tokens redeemed and collected. |
| ❎ | `ulong` | VestedBlock | The block number that the certificate was fully vested at. |

---

### Revoke Vault Certificate Log

Emitted when the vault owner revokes a non-vested wallet's certificate.

| Index | Type | Property | Description |
| ✅ | `Address` | Owner | The address of the holder of the revoked certificate. |
| ❎ | `UInt256` | OldAmount | The old amount of tokens the certificate was created for. |
| ❎ | `UInt256` | NewAmount | The new amount of tokens the vault certificate can be redeemed for. |
| ❎ | `ulong` | VestedBlock | The block number that the certificate will be eligible for redemption at. |

---

### Create Vault Proposal Log

Emitted after a new proposal is created.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The ID of the proposal created. |
| ✅ | `Address` | Wallet | The address of the wallet affected by the proposal. For create certificate proposals, this is the recipient. For revoke certificate proposals, this is the current holder. For minimum total proposals, this is the creator. |
| ❎ | `UInt256` | Amount | The amount of the proposal. For new certificates this is the amount proposed to be assigned, for revoke certificates, this is the current certificate's amount, for change total proposals, this is the new total proposed. |
| ❎ | `byte` | Type | The type of proposal. |
| ❎ | `byte` | Status | The status of the proposal. |
| ❎ | `ulong` | Expiration | The expiration block of the current status of the proposal. |
| ❎ | `string` | Description | The description of the proposal. |

---

### Complete Vault Proposal Log

Emitted after a proposal is expired and completed.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The ID of the proposal completed. |
| ❎ | `bool` | Approved | True if the proposal was approved, false if it was not. |

---

### Vault Proposal Pledge Log

Emitted after pledging toward a proposal.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The proposal ID pledged to. |
| ✅ | `Address` | Pledger | The wallet address of the pledger. |
| ❎ | `ulong` | PledgeAmount | The amount of CRS pledged to the proposal in this transaction. |
| ❎ | `ulong` | PledgerAmount | The total CRS balance of the pledger for this proposal. |
| ❎ | `ulong` | ProposalPledgeAmount | The total amount of CRS pledged to this proposal. |
| ❎ | `bool` | TotalPledgeMinimumMet | True if the pledge has met its minimum requirement, moving it on to the vote stage. |

---

### Vault Proposal Vote Log

Emitted after voting on a proposal.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The proposal ID voted on. |
| ✅ | `Address` | Voter | The wallet address of the voter. |
| ❎ | `bool` | InFavor | True if the vote was in favor of the proposal, else false. |
| ❎ | `ulong` | VoteAmount | The amount of CRS used to vote in this transaction. |
| ❎ | `ulong` | VoterAmount | The total amount of CRS the voter has voted with on this proposal. |
| ❎ | `ulong` | ProposalYesAmount | The total amount CRS voted Yes on the proposal. |
| ❎ | `ulong` | ProposalNoAmount | The total amount CRS voted No on the proposal. |

---

### Vault Proposal Withdraw Pledge Log

Emitted after withdrawing locked CRS used to pledge to a proposal.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The proposal ID the CRS were withdrawn from. |
| ✅ | `Address` | Pledger | The wallet address of the pledger. |
| ❎ | `ulong` | WithdrawAmount | The amount of CRS to withdraw from a previous pledge to this proposal. |
| ❎ | `ulong` | PledgerAmount | The total amount of CRS the pledger still has locked within the proposal. |
| ❎ | `ulong` | ProposalPledgeAmount | The total amount of CRS pledged to the proposal. |
| ❎ | `bool` | PledgeWithdrawn | True if the funds withdrawn were deducted from an in progress proposal in the pledge stage, else false. |

---

### Vault Proposal Withdraw Vote Log

Emitted after withdrawing locked CRS used to vote on a proposal.

| Index | Type | Property | Description |
| ✅ | `ulong` | ProposalId | The proposal ID the CRS were withdrawn from. |
| ✅ | `Address` | Voter | The wallet address of the voter. |
| ❎ | `ulong` | WithdrawAmount | The amount of CRS to withdraw from a previous vote to this proposal. |
| ❎ | `ulong` | VoterAmount | The total amount of CRS the voter still has locked within the proposal. |
| ❎ | `ulong` | ProposalYesAmount | The total amount CRS voted Yes on the proposal. |
| ❎ | `ulong` | ProposalNoAmount | The total amount CRS voted No on the proposal. |
| ❎ | `bool` | VoteWithdrawn | True if the funds withdrawn were deducted from an in progress proposal in the vote stage, else false. |

---

## References

#### OpdexVault Smart Contract - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Contracts/OpdexVault.cs" target="_blank">Github</a>

#### IOpdexVault Interface - <a href="https://github.com/Opdex/opdex-governance/blob/main/src/Interfaces/IOpdexVault.cs" target="_blank">Github</a>

#### Stratis SmartContract Documentation - <a href="https://academy.stratisplatform.com/Architecture%20Reference/SmartContracts/working-with-contracts.html" target="_blank">Stratis Academy</a>
