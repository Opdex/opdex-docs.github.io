---
layout: page
title: Platform API
has_children: true
nav_order: 5
---

Learn the technical details about how the Opdex platform API is setup and ran.

The Opdex Platform Web API is an interface for querying information from the Opdex contract indexer and quoting Opdex smart contract transactions. The Opdex contract indexer is a job that continually runs and stores relevant broadcast transaction data, in a way that represents the current known state of the Opdex protocol at the present time.

[View the Github Repository](https://github.com/opdex/opdex-v1-api)

---

## Basics

The Web API is a set of RESTful HTTP endpoints. When calling endpoints, HTTPS should be used. Methods that accept a request body will only accept parameters passed in JSON format.

## Maintenance

In rare circumstances, such as during a new deployment, the Web API may be configured to be under maintenance. When this happens is determined by whoever is hosting the Web API. During these times, any endpoint that relies on indexed data will return a HTTP status code of 503 Service Unavailable. It is recommended to handle this, as well as all other non-successful status codes that are documented for each endpoint.

## Paginated Endpoints

Some endpoints provide access to ever-increasing large datasets. For this reason, we use cursor-based pagination to make the responses easier to handle. Results can be filtered through the inclusion of various parameters in the query string.

All paginated endpoints return a paging object in the response, which can contain a next and/or a previous base-64 encoded string, the cursor. This cursor can be passed to the same endpoint via the query string, to retrieve the next or previous results. The cursor contains information about the filters that were passed in the first request, therefore there is no need to pass additional filters into subsequent requests.

## Null Values

Any null values passed into a request will be treated as such.

Null values will never be present in a response as they will be left out. A value may be null where the meaning is that it is not set. Objects and arrays with no values will be treated as such, meaning they will be present in the response and be empty. Numbers with no value will be treated as 0 and strings with no value will be treated as empty strings.

## Token Amounts

Any token amounts that are passed to a request are in the decimal form and must contain a decimal point. For example, if you wanted to pass an amount of 10 CRS into a request, it should be formatted as `"10.00000000"`. Requiring the decimal point is intended a safety measure, so that it is far less likely to mistakenly create quotes with unintended large token amounts.

Token amounts that are returned in a response will always be in decimal form as well.

## Decimal Numbers

Any decimal numbers passed into a request must be passed as a string. Decimal numbers are also returned as string values, with full precision.

## Date and Time

All date and time data uses ISO-8601 as a formatting standard. Date and time data sent in a request should specify the offset, otherwise it will be assumed that the time zone is UTC. For example, if you wish to specify a time of `2022-01-01 06:00:00 PST`, you can format this as `2022-01-01T06:00:00-08:00`. All date and time data that is returned is UTC and will be expressed as such.