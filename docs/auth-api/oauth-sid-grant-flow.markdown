---
layout: page
title: OAuth SID Grant Flow
parent: Auth API
nav_order: 1
---

## Authorization Request

The client constructs the request URI by adding the following parameters to the query component of the authorization endpoint URI using the "_application/x-www-form-urlencoded_" format, per [Appendix B](https://datatracker.ietf.org/doc/html/rfc6749#appendix-B) of RFC 6749:

| Key           | Required | Description                      |
| :------------ | :------- | :------------------------------- |
| response_type | Yes      | Value **MUST** be set to "_sid_" |

The client makes a request to the constructed URI.

For example, the client makes the following HTTP request using TLS:

```http
GET /authorize?response_type=sid HTTP/1.1
Host: server.example.com
```



The authorization server validates the request to ensure that all required parameters are present and valid.

## Authorization Response

The authorization server issues a Stratis Id URI and delivers it to the client by returning a response code of 200 OK, using the “_text/plain_” format. An example successful response:

```http
HTTP/1.1 200 OK
Content-Type: text/plain

"sid:app.opdex.com/v1/ssas?uid=KI1VrzERA5mbGb6irCLmIn-T2HmBe0YxhdcxP9pbEF_Ii9gVmPSw-LtIatqKhhXzlD3-lFcD38-LKlvuNdcjug&exp=1651235800"
```



## Access Token Request

The client makes a request to the token endpoint by adding the following parameters using the "_application/x-www-form-urlencoded_" format per [Appendix B](https://datatracker.ietf.org/doc/html/rfc6749#appendix-B) of RFC 6749 with a character encoding of UTF-8 in the HTTP request entity-body:

| Key        | Required | Description                                                    |
| :--------- | :------- | :------------------------------------------------------------- |
| grant_type | Yes      | Value **MUST** be set to "_sid_"                               |
| sid        | Yes      | The Stratis ID string received from the authorization response |
| public_key | Yes      | Blockchain address used to create the signature                |
| signature  | Yes      | Signed Stratis ID                                              |

For example, the client makes the following HTTP request using TLS:

```http
POST /token HTTP/1.1
Host: server.example.com
Content-Type: application/x-www-form-urlencoded

grant_type=sid&sid=sid:app.opdex.com/v1/ssas?uid=KI1VrzERA5mbGb6irCLmIn-T2HmBe0YxhdcxP9pbEF_Ii9gVmPSw-LtIatqKhhXzlD3-lFcD38-LKlvuNdcjug&exp=1651235800&public_key=&signature=
```



The authorization server **MUST**:

- validate the validity of the stratis ID
- verify that the public_key is a valid address
- verify that the signature is valid
- ensure that the token is issued if the signature can be verified

## Access Token Response

If the access token request is valid and authorized, the authorization server issues an access token and optional refresh token as described in [Section 5.1](https://datatracker.ietf.org/doc/html/rfc6749#section-5.1) of RFC 6749.  If the request client authentication failed or is invalid, the authorization server returns an error response as described in [Section 5.2](https://datatracker.ietf.org/doc/html/rfc6749#section-5.2) of RFC 6749.

An example successful response:

```http
HTTP/1.1 200 OK
Content-Type: application/json;charset=UTF-8
Cache-Control: no-store
Pragma: no-cache

{
  "access_token":"2YotnFZFEjr1zCsicMWpAA",
  "token_type":"example",
  "expires_in":3600,
  "refresh_token":"tGzv3JOkF0XG5Qx2TlKWIA",
  "example_parameter":"example_value"
}
```
