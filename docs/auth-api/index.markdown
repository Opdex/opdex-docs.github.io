---
layout: page
title: Auth API
has_children: true
nav_order: 6
---

Learn the technical details about how the Opdex Auth API is setup and run.

The Opdex Auth Web API is an interface for connecting wallets to Cirrus dApps. By integrating with the API, dApps can allow users to connect their Cirrus wallet through an OAuth2 flow. Any Cirrus wallet software can support this flow with a simple callback.

[View the Github Repository](https://github.com/opdex/auth-api)

---

## Basics

When calling endpoints, HTTPS should be used.

## Authentication Methods

### Delegating to a User

You can connect users to your dApp by using the authorization code PKCE grant. This delegates authentication to the user of your application. It is recommended to use an OAuth2 client package that supports PKCE, to integrate with the Opdex Auth Web API, although it can also be done by forming requests yourself, albeit with a little extra effort.

### Machine to Machine (M2M)

It is possible to perform machine-to-machine authentication using the custom [sid grant](oauth-sid-grant-flow). Common use cases for this include authenticating a daemon/service, bot or IoT device. The requirement for this is that the application being integrated is able to sign messages with their Cirrus wallet, ideally using their own node.

## Integrating a Wallet

Any Cirrus wallet software can act as an OAuth2 resource owner and be used to authenticate users for all OAuth2 clients, by implementing [SSAS](https://github.com/Opdex/SSAS). The implementation relies on a standard HTTP callback at `/ssas/callback` which may be directed to the Opdex Auth API.

## Swagger

A list of available endpoints, requests and responses are documented within `/swagger` of the running API.
