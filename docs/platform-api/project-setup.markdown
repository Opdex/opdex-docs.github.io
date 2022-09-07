---
layout: page
title: Project Setup
parent: Platform API
nav_order: 0
---

## Platform API Project Setup

Get started setting up your Opdex Platform API environment.

### Dependencies

#### MySql

The Platform API uses MySql v8.0 databases for its operations. This can be setup locally or hosted.

#### Cirrus Full Node

A Cirrus Full node with Address Indexing is required to facilitate the necessary queries to support the Platform UI client. Address Indexing allows the retrieval of CRS balances.

**Devnet**

Use the `Stratis.CirrusMinerD` project to run the full node with `dotnet run -devmode=miner -addressindex`

**Testnet or Mainnet**

Use the `Stratis.CirrusD` project to run the full node with `dotnet run -addressindex` and optionally with `-testnet`.

#### Azure SIgnalR Service

An Azure SIgnalR service is required currently and is used to push transaction based notifications to connected clients through web sockets. 

#### Coin Market Cap

Coin Market Cap API is used to synchronize STRAX USD prices in order to calculate the rest of the API's USD prices. Currently DEVNET makes a request every 15 minutes, TESTNET and MAINNET are every minute, so a paid tier is required.

---

### Configurations

Configurations and secrets need to be set according to the environment and network you want to run the API in. 

Create your environment secrets file using `dotnet user-secrets set <key> <value>` for each of the properties below or create the JSON file yourself. 

#### Database Configuration

- `ConnectionString` - Used to connect to an environment's Platform DB.

#### Opdex Configuration

- `Network` - DEVNET, TESTNET, or MAINNET
- `ApiUrl` - The domain that will be hosting this API used for client callbacks.

#### Auth Configuration

- `Opdex:SigningKey` - JWT signing key (UTF-8 string 16 bytes or more)
- `AdminKey` - Admin access key

#### Encryption Configuration

- `Key` - Encryption key (UTF-8 string 32 bytes)

#### Coin Market Cap Configuration

- `ApiKey` - Coin Market Cap API Key

#### Cirrus Configuration

- `ApiUrl` - The URL of the hosted full node.
- `ApiPort` - The port of the full node to call.

#### Azure

- `SignalR:ConnectionString` - Azure service connection string to push web notifications.

Create your environment secrets file using `dotnet user-secrets set <key> <value>` for each of the properties below or create the JSON file yourself.

```JSON
{
  "DatabaseConfiguration:ConnectionString": "",
  "OpdexConfiguration:Network": "",
  "OpdexConfiguration:ApiUrl": "",
  "AuthConfiguration:Opdex:SigningKey": "",
  "AuthConfiguration:AdminKey": "",
  "EncryptionConfiguration:Key": "",
  "CoinMarketCapConfiguration:ApiKey": "",
  "CirrusConfiguration:ApiUrl": "",
  "CirrusConfiguration:ApiPort": "",
  "Azure:SignalR:ConnectionString": ""
}
```
