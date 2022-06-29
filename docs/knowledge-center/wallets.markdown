---
layout: page
title: Wallets
parent: Knowledge Center
nav_order: 0
---

Wallet support includes but may not be limited to Cirrus Core desktop wallet and the Stratis Mobile Wallet, both of which can be found at <a href="https://www.stratisplatform.com/wallets/" target="_blank">https://www.stratisplatform.com/wallets</a>.

Opdex is non-custodial, meaning there is no central authority where tokens are deposited similar to how centralized exchange services work. Your wallet and tokens are under your control, so it is critical to securely backup your wallet, so that it can be restored if the need arises.

The primary wallet functions needed for Opdex compatibility are limited to logging in (SSAS) and transaction handoff (STHS).

{: .warning }
Your wallet is your own responsibility. There is nobody that can access or replace lost or stolen funds, so it is critical to secure your wallet and **NEVER** give out your seed phrase or private keys.

## Logging In

Logging into a dApp is a simple process where you prove ownership of your wallet, in a trustless way, without sharing personal information. In short, a QR code message is generated, you sign that message using your wallet, then the wallet sends the signature back. This allows dApps to verify that the message was correctly signed, and assign you an access token.

### 1. Get Auth Code

Navigate to <a href="https://app.opdex.com/auth" target="_blank">Opdex login view</a> to retrieve a QR code message.

![Login Screen](baf867e-Login.jpg)

### 2. Scan or Copy QR Code to Wallet

Scan or copy the QR code into your Cirrus wallet, accepting any prompt to login or sign the message.

![Wallet Login Views](3cb5086-WalletLogin.jpg)

### 3. Return to Opdex

Upon successful authentication, the Opdex UI will redirect you. The wallet navigation link will now have a green indicator, showing that you are logged in.

---

## Submitting Transactions

The process of submitting a transactions is very similar to logging in. A dApp will show a QR code containing the transaction info, such as the smart contract method, parameters, and transaction amounts. These QR codes contain the necessary information for your wallet to build and execute the actual transaction.

### 1. Prepare a Transaction

For any transaction that you want to make, fill out the required fields. A popup will display the transaction details, along with a QR code to scan or copy it to your wallet.

![Transaction Quote](dfd717f-TransactionQuote.jpg)

### 2. Scan or Copy QR Code to Wallet

Scan or copy the QR code into a Cirrus wallet to review, then submit the transaction.

![Wallet Transaction Views](348598d-WalletTransaction.jpg)

### 3. Return to Opdex

Upon a successful transaction broadcast, the Opdex UI will display a notification. Another notification will be displayed once the transaction is mined.
