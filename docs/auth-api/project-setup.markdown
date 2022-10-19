---
layout: page
title: Project Setup
parent: Auth API
nav_order: 0
---

# Auth API Project Setup

Get started setting up your Opdex Auth API environment.

## Dependencies

### MySql

The Platform API uses MySql v8.0 databases for its operations. This can be setup locally or hosted.

Schema and scripts for database management can be found in the [Opdex Auth DB Repository](https://github.com/Opdex/opdex-auth-db)

### Cirrus Full Node

A Cirrus Full node is required to facilitate the message signature validations required to verify a user has properly signed auth messages.

#### Devnet

Use the `Stratis.CirrusMinerD` project to run the full node with `dotnet run -devmode=miner`

#### Testnet or Mainnet

Use the `Stratis.CirrusD` project to run the full node with `dotnet run` and optionally with `-testnet`.

### Azure SignalR Service

An Azure SIgnalR service is required currently and is used to push auth tokens back to clients who have successfully signed an authorization message.

---

## Configurations

Configurations and secrets need to be set according to the environment and network you want to run the API in.

Create your environment secrets file using `dotnet user-secrets set <key> <value>` for each of the properties below or create the JSON file yourself.

- `Authority` - Hosted Auth API URL
- `Azure:SignalR:ConnectionString` - Connection string to Azure SignalR
- `Cirrus:ApiUrl` - URL to hosted Cirrus node.
- `Database:ConnectionString` - Connection string to database.
- `Encryption:Key` - Secret encryption key.
- `IpRateLimiting:IpWhitelist:1` - Optional rate limiting IP whitelist.
- `Jwt:SigningKeyName` - Any string, consider "opdex"
- `Jwt:SigningKeyVersion` - Secret version key (GUID)
- `Prompt` - Auth client URL for redirects.

### Example Configuration

```json
{
    "Authority": "auth-api.opdex.com",
    "Azure:SignalR:ConnectionString": "Endpoint=https://ws-opdex.service.signalr.net;AccessKey=1468d92882f6499797735a9d5c07ae562NeZvQHH7lN=;Version=1.0;",
    "Cirrus:ApiUrl": "https://cirrus.opdex.com",
    "Database:ConnectionString": "Server=sql-opdex.mysql.database.azure.com; Port=3306; Database=auth; Uid=auth@sql-opdex; Pwd=abcdefg;",
    "Encryption:Key": "H,|!<8X(EfBb(}KT",
    "IpRateLimiting:IpWhitelist:1": "192.168.0.1",
    "Jwt:SigningKeyName": "opdex",
    "Jwt:SigningKeyVersion": "51d51483ec3947318877a8571cddff98",
    "Prompt": "https://auth.opdex.com"
}
```
