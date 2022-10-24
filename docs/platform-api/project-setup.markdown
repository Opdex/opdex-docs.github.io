---
layout: page
title: Project Setup
parent: Platform API
nav_order: 0
---

# Platform API Project Setup

Get started setting up your Opdex Platform API environment.

## Dependencies

### MySql

The Platform API uses MySql v8.0 databases for its operations. This can be setup locally or hosted.

Schema and scripts for database management can be found in the [Opdex V1 Db Repository](https://github.com/opdex/opdex-v1-db)

### Cirrus Full Node

A Cirrus Full node with Address Indexing is required to facilitate the necessary queries to support the Platform UI client. Address Indexing allows the retrieval of CRS balances.

#### Devnet

Use the `Stratis.CirrusMinerD` project to run the full node with `dotnet run -devmode=miner -addressindex -txindex`

#### Testnet or Mainnet

Use the `Stratis.CirrusD` project to run the full node with `dotnet run -addressindex -txindex` and optionally with `-testnet`.

### Azure SignalR Service

An Azure SIgnalR service is required currently and is used to push transaction based notifications to connected clients through web sockets. 

### Coin Market Cap

Coin Market Cap API is used to synchronize STRAX USD prices in order to calculate the rest of the API's USD prices. Currently DEVNET makes a request every 15 minutes, TESTNET and MAINNET are every minute, so a paid tier is required.

---

## Configurations

Configurations and secrets need to be set according to the environment and network you want to run the API in. 

Create your environment secrets file using `dotnet user-secrets set <key> <value>` for each of the properties below or create the JSON file yourself. 

### Auth Configuration

- `Opdex:SigningKey` - JWT signing key (UTF-8 string 16 bytes or more)
- `AdminKey` - Admin access key
- `Issuer` - API url issuer of JWT (auth server).
- `StratisSignatureAuth:CallbackPath` - Auth endpoint callback path.

### Azure

- `SignalR:ConnectionString` - Azure service connection string to push web notifications.

### Cirrus Configuration

- `ApiUrl` - The URL of the hosted full node.
- `ApiPort` - The port of the full node to call.

### Coin Market Cap Configuration

- `ApiKey` - Coin Market Cap API Key
- `ApiUrl` - The API url for CMC.
- `HeaderName` - Coin Market Cap required API Key header.

### Database Configuration

- `ConnectionString` - Used to connect to an environment's Platform DB.

### Encryption Configuration

- `Key` - Encryption key (UTF-8 string 32 bytes)

### Feature Management

- `AuthServer` - True or False for auth and JWT handling.
- `CoinMarketCapPriceFeed` - True or False, false falls back on Coin Gecko checking pricing less frequently.

### Maintenance

- `Locked` - True or False, locks the API from requests during maintenance.

### Opdex Configuration

- `Network` - DEVNET, TESTNET, or MAINNET
- `ApiUrl` - The domain that will be hosting this API used for client callbacks.
- `WalletTransactionCallback` - The endpoint to use when a wallet broadcasts a transaction to the blockchain.

### Example Configuration

```json
{
  "AuthConfiguration:AdminKey": "fd45866cbc914d9e862e99240c6add99",
  "AuthConfiguration:Issuer": "auth-api.opdex.com",
  "AuthConfiguration:Opdex:SigningKey": "tadRyudfrW7DEHedZn8R6eMmcoYQF2GceVg1LqLAFsnzeKfngGyeTi7w4PZzBC69bPkLSyqqLqQOnjHIqGlj1OVZI9NSPSmjm1cBnEeUuJlapFNC0wa3w0g8wgiokoyl7TmFTulExz5zTDQI57329ZSrOv3sXEZpfjeF24EKa8iiSEVF1i2P81YNZeRs9Uuajvoac04uYx835sWUeguhlwedGJr03MHLNmnxIsT82HKntGq5FMbGr4BeKyMS7cznr354RJBN5vs5dW4bb70ldPSs5PTh8d1s3nnMxYDQDtLW9SrzFqBXjzzefJPCcTpfsPuevu392ULhGtrm9Ms7dJ1kfZgWIT6lrCpef1WvLxDy12ZusHcAcel92GeuyXlJKGyPHWuZbWPNDXAP2UJIlqLIBtW7RdZ8Sa1g8Tvf9izA2hBEcC6azAYqIF8tyIqs3IHSMKfVjRMbqMFWIvzeWIBtHmx5FBgkJanu2JJYW0fWpgVRxbvMMl8jRsHJMpT0WtFmFUtWCXEkhid0iulTitcsLg3Pb8x83IXOqpw6ruE9bpjClMifUvKMETYwLyjZYplAsA2PJ4tIw7NPs8SmD3NOje77qf5pgf3Lac2pUIlEPIodH2dRXdLktllNb6dX7MA1flqYsAaOqBQXrTcEcuPkT4zWLZYlCp38sKAvGvTAPOcDWcqmL0jb3zgmqHLcVaNUbPzdbriXHfd0vv9XYmU5BdmxPR7TqyoR7qidVzLdrmEhRGUULtxzd8aoTc4yuKIHbGKXcc4t783GBdXovDd8iiZosDEz7ludH06T4l7X",
  "AuthConfiguration:StratisSignatureAuth:CallbackPath": "auth",
  "Azure:SignalR:ConnectionString": "Endpoint=<signalr-endpoint>;AccessKey=<access-key>;Version=1.0;",
  "CirrusConfiguration:ApiPort": "0",
  "CirrusConfiguration:ApiUrl": "https://cirrus.example.com",
  "CoinMarketCapConfiguration:ApiKey": "0cc23bbd5496430d9f7b5efd659e008c",
  "CoinMarketCapConfiguration:ApiUrl": "https://pro-api.coinmarketcap.com/v2/cryptocurrency/",
  "CoinMarketCapConfiguration:HeaderName": "X-CMC_PRO_API_KEY",
  "DatabaseConfiguration:ConnectionString": "Server=<server>; Port=3306; Database=<db-name>; Uid=<id>; Pwd=<pwd>",
  "EncryptionConfiguration:Key": "H,|!<8X(EfBb(}KT",
  "FeatureManagement:AuthServer": "True",
  "FeatureManagement:CoinMarketCapPriceFeed": "True",
  "IpRateLimiting:IpWhitelist:1": "192.168.0.1",
  "MaintenanceConfiguration:Locked": "False",
  "OpdexConfiguration:ApiUrl": "https://v1-api.opdex.com/v1",
  "OpdexConfiguration:Network": "MAINNET",
  "OpdexConfiguration:WalletTransactionCallback": "/transactions"
}
```
