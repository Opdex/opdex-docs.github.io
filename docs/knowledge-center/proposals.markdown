---
layout: page
title: Proposals
parent: Knowledge Center
nav_order: 10
---

Vault proposals allow CRS token holders to vote on the allocation of governance tokens in the vault, as well as the requirements of the voting process itself.

## Creating a Proposal

There are four types of proposals that can be made:

- Create Certificate
- Revoke Certificate
- Minimum Pledge
- Minimum Vote

To successfully create a proposal, it is required to provide a 500 CRS refundable deposit to the vault. This deposit is returned to the creator of the proposal, once the proposal is complete. Every proposal requires that the creator provides a description. Due to the 200 character limit of the proposal description, it is recommended to provide a link to a document, that explains the proposal in more detail. The more information included in the proposal the better, as this is what voters will use to decide the outcome of the proposal.

### Create Certificate

Tokens held in the vault can be distributed to individuals, through a successful proposal to create a new certificate. To create a proposal for a new certificate, you must provide a token value for the certificate less than or equal to the total supply in the vault, as well as a recipient address for the certificate.

### Revoke Certificate

If you believe that a certificate should no longer be accumulating value while vesting, for example due to false information provided in the original proposal, it is possible to get it revoked. Creating a proposal to revoke a certificate requires the address of the certificate holder.

### Minimum Pledge

A minimum pledge proposal can be created to change the CRS pledge requirement that a proposal must meet, to be moved on to a vote. Creating a proposal to change the minimum pledge requires a CRS amount to be specified for the new minimum amount. A minimum pledge proposal requires the existing CRS minimum pledge amount to be able to move on to a vote.

### Minimum Vote

A minimum vote proposal can be created to change the CRS vote requirement that a proposal must meet, to be able to successfully pass. Creating a proposal to change the minimum vote requires a CRS amount to be specified for the new minimum amount. A minimum vote proposal requires the existing CRS minimum vote amount to be pass.

## Tips for a Successful Proposal

1. Be detailed. Include a link to a document in the proposal description, that contains everything that a voter would need to know about the proposal to make an informed decision.
2. Provide a strong case for the proposal. Explain why it is necessary, or would benefit token holders.
3. Be truthful and provide factual information. False claims will look bad and voters will not trust you.
4. Keep it realistic. Ensure that you can achieve any goals set out in the proposal.
5. Take steps to allow voters to verify information provided in the proposal. For example, sign a message with an address that you claim to control.
6. Make the request reasonable. It is highly unlikely that anyone will vote in favour for the entire vault supply to be assigned to a single certificate.
7. Do not edit the proposal. Prepare the proposal ahead of time, as any edits made to the proposal details could cause voters to lose trust. If you want to change a proposal based on feedback, wait for the original proposal to fail, then create a new proposal.
8. Make yourself known. Coming into the community and creating a proposal as anon does not make you look trustworthy. Establish an online presence and get to know community members and token holders.

## Voting On a Proposal

There are 2 stages to a proposal, a pledge stage and a vote stage. Both stages have limited windows of participation and both have minimum limits of total votes required as set by the [vault](Vault).

### Pledge Stage

The pledge stage is the first stage of a proposal and can last a maximum of 7 days before being denied. During this stage, community members should review the proposal, discuss amongst each other, and pledge using CRS if they are in favor of the proposal. If the proposal meets the minimum pledge requirement then it immediately moves on to the vote stage.

### Vote Stage

The vote stage is when votes must be cast in favor or opposition of the proposal by using CRS tokens as vote weight. The period lasts for 3 days (16,200 blocks) then prevents further votes from being cast. At the end of the vote period, the total vote weight must meet the minimum vote requirement or the proposal will be denied. In addition to the minimum requirement, more votes must be in favor than in opposition for a proposal to succeed.

### Completion

If a proposal is successful, it will execute the proposed action such as creating a certificate or modifying the minimum vote amount. Proposals that are denied will not execute the proposed action and will be locked from further participation. At this time, the initial 500 CRS the creator deposited for the proposal will be returned to them.

#### Withdrawing Pledges and Votes

Withdrawing pledges and votes can be done at anytime but doing so may deduct from the proposals totals. For example, withdrawing a vote in favor of the proposal when it is still within the 3 day voting period will deduct the user's vote from the number of in favor votes.
