# Error Codes and Common Fixes

Every error the API can return, with the cause and the fix. The four most common are first. The full list follows, grouped by HTTP status.

## Response shape

Every response carries a numeric `code` and a `message`. A successful request returns HTTP 200 with `code` 2000. An error returns the matching HTTP status and a code that starts with that status.

```json
{
  "code": 4010,
  "message": "Invalid Key. For more information see http://ideal-postcodes.co.uk/documentation/response-codes#4010"
}
```

Check `code`, not the message text. Some messages carry a trailing link to a legacy response codes page. The anchors on that page match the anchors here.

Two errors add a field:

- Request validation failures (`code` 4000 on the validated endpoints) add `errors`, an array of `{ path, message }` naming each invalid field.
- A postcode that does not exist (`code` 4040 on `/v1/postcodes/:postcode`) adds `suggestions`, an array of nearby valid postcodes.

With a JSONP `callback` parameter the API returns every error with HTTP 200, so read `code` from the body rather than relying on the status.

## Summary

| Code | HTTP | Meaning |
|---|---|---|
| [4000](#4000) | 400 | Invalid syntax or failed request validation |
| [4001](#4001) | 400 | Submitted data failed validation |
| [4005](#4005) | 400 | Invalid end date |
| [4006](#4006) | 400 | Invalid start date |
| [4007](#4007) | 400 | Start date is after end date |
| [4008](#4008) | 400 | Date range over 90 days |
| [4009](#4009) | 400 | More than 3 tags queried |
| [40010](#40010) | 400 | Invalid source IP address |
| [40011](#40011) | 400 | Invalid search query |
| [40012](#40012) | 400 | Pagination beyond 10,000 results |
| [40013](#40013) | 400 | Too many biases |
| [40014](#40014) | 400 | Too many filters |
| [40016](#40016) | 400 | Invalid filter or bias value |
| [40017](#40017) | 400 | Email query too long |
| [40018](#40018) | 400 | Missing query |
| [4010](#4010) | 401 | Invalid key |
| [4011](#4011) | 401 | URL or IP not on allowed list, or missing user token |
| [4012](#4012) | 401 | Key not owned by user token |
| [4013](#4013) | 401 | Sub-licensee key required |
| [4014](#4014) | 401 | Licensee belongs to another key |
| [4015](#4015) | 401 | Key not licensed for this data |
| [4016](#4016) | 401 | Invalid context |
| [4020](#4020) | 402 | Balance depleted |
| [4021](#4021) | 402 | Lookup limit reached |
| [404](#404) | 404 | Page not found |
| [4040](#4040) | 404 | Postcode not found |
| [4042](#4042) | 404 | Key not found |
| [4044](#4044) | 404 | UDPRN not found |
| [4045](#4045) | 404 | Licensee not found |
| [4046](#4046) | 404 | UMPRN not found |
| [4047](#4047) | 404 | Config not found |
| [4048](#4048) | 404 | Address not found |
| [4100](#4100) | 410 | Signup link expired |
| [4150](#4150) | 415 | Unsupported media type |
| [4290](#4290) | 429 | Request timed out |
| [4291](#4291) | 429 | Too many requests |
| [5001](#5001) | 500 | Uncatalogued error |
| [5002](#5002) | 500 | Internal timeout |

### 4010 - Invalid Key {#4010}

**HTTP 401.** Message: `Invalid Key`

Your API Key was not recognised. The key may be incorrect, malformed or deleted. On the key management endpoints (`/v1/keys/:key/*`) this also means your `user_token` does not own the key.

#### Potential fixes {#fixes-4010}

1. **Check for typos** - copy the key directly from your dashboard.
2. **Check querystring parameter name** - ensure the key is passed as `api_key` and not `api-key`.
3. **Check Authorization header format** - ensure the header is formatted as `IDEALPOSTCODES api_key="ak_yourkey"`.
4. **Check the key still exists** - a regenerated or deleted key stops working immediately.

### 4011 - URL Not on Allowed List {#4011}

**HTTP 401.** Message: `Requesting URL not on whitelist` or `Forbidden`

`Requesting URL not on whitelist` means the request's `Referer` or `Origin` header did not match any URL on your key's [allowed URL list](https://docs.ideal-postcodes.co.uk/docs/guides/allowed-urls). Sub-licensee keys apply their own allowed URLs in the same way.

`Forbidden` means one of two things. The request came from an IP address that is not on the key's IP allow list. Or a key management endpoint (`/v1/keys/:key/details`, `/usage`, `/lookups`, `/licensees`, `/configs`) received no `user_token`, or one it did not recognise.

#### Potential fixes {#fixes-4011}

1. **Check if you need Allowed URLs** - non-browser requests won't contain the `Referer` or `Origin` headers needed for matching. Remove Allowed URLs if the key is kept private.
2. **Review your Allowed URL configuration** - check the [API Key security guide](https://docs.ideal-postcodes.co.uk/docs/guides/api-key-secure) to ensure URLs have been defined correctly.
3. **Check the IP allow list** - if your key restricts IP addresses, add the address your servers send from.
4. **Send a valid user token** - key management endpoints need `user_token` as well as the key.

### 4020 - Balance Depleted {#4020}

**HTTP 402.** Message: `Key balance depleted`

Your API Key has no remaining lookup balance.

#### Potential fixes {#fixes-4020}

1. **Top up your balance** - buy more lookups from your dashboard.
2. **Enable automated top-ups** - prevent this from recurring by [enabling automated top-ups](https://docs.ideal-postcodes.co.uk/docs/guides/automated-topups).

### 4021 - Lookup Limit Reached {#4021}

**HTTP 402.** Message: `Lookup Limit Reached`

Your API Key has a limit configured and the request would exceed it. Four limits raise this error: the daily lookup limit, the monthly lookup limit, the individual (per IP address) daily limit and a sub-licensee's daily limit.

#### Potential fixes {#fixes-4021}

1. **Disable the rate limit** - remove the responsible limit in your [key settings](https://docs.ideal-postcodes.co.uk/docs/guides/api-key-settings) for an immediate fix.
2. **Increase the limit** - adjust the daily, monthly or individual limit in your key settings.
3. **Forward the end user's IP** - if requests reach us through a proxy, one address absorbs every user's lookups and trips the individual limit early. Enable IP Address Forwarding and send `IDPC-Source-IP`.
4. **Check the licensee's limit** - a sub-licensee key carries its own daily limit, separate from the parent key.

### 4000 - Invalid Syntax {#4000}

**HTTP 400.** Message: `Invalid syntax submitted`, or a validation message such as `request/query/query must be string`

The API could not parse the request or the request failed validation. Causes:

- A POST or PUT body that is not valid JSON.
- Malformed percent-encoding in the URL.
- A missing or non-numeric `lonlat` on `/v1/postcodes`, or a non-integer `limit` or `radius`.
- A non-numeric UDPRN or UMPRN in the path.
- A failed schema check on `/v1/emails`, `/v1/phone_numbers`, `/v1/cleanse/addresses`, `/v1/sign_up` or the `/v1/keys/:key/details` and `/v1/keys/:key/configs` endpoints. These responses include an `errors` array naming each invalid field.

Fix the request body or parameter named in `message` or `errors`.

### 4001 - Validation Failed {#4001}

**HTTP 400.** Message: `Validation failed on your submitted data` or a field-specific message

A write to your key failed validation. Raised by `PUT /v1/keys/:key/details`, the licensee create and update endpoints and the config create and update endpoints. An invalid monthly limit value also raises it. Check the field named in the message against the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/key-details).

### 4005 - Invalid End Date {#4005}

**HTTP 400.** Message: `Invalid end date`

The `end` parameter on `/v1/keys/:key/usage` or `/v1/keys/:key/lookups` could not be parsed. Send an ISO 8601 date.

### 4006 - Invalid Start Date {#4006}

**HTTP 400.** Message: `Invalid start date`

The `start` parameter on `/v1/keys/:key/usage` or `/v1/keys/:key/lookups` could not be parsed. Send an ISO 8601 date.

### 4007 - Start After End {#4007}

**HTTP 400.** Message: `Invalid Date Range: start date is after end date`

On `/v1/keys/:key/usage` or `/v1/keys/:key/lookups`, `start` is later than `end`. Swap them.

### 4008 - Range Too Wide {#4008}

**HTTP 400.** Message: `Invalid Date Range: range specified needs to be 90 days or less`

The `start` to `end` window on `/v1/keys/:key/usage` or `/v1/keys/:key/lookups` exceeds 90 days. Split the query into 90-day windows.

### 4009 - Too Many Tags {#4009}

**HTTP 400.** Message: `Too Many Tags Queried: please specify no more than 3 tags to query`

`/v1/keys/:key/usage` accepts at most three `tags`. Query fewer tags per request.

### 40010 - Invalid Source IP {#40010}

**HTTP 400.** Message: `Invalid source IP address provided`

The `IDPC-Source-IP` header does not contain a valid IP address and your key has an individual lookup limit configured. Send a single valid IP address, or drop the header. See [IP Address Forwarding](https://docs.ideal-postcodes.co.uk/docs/guides/api-key-settings#ip-address-forwarding).

### 40011 - Invalid Search Query {#40011}

**HTTP 400.** Message: `Invalid search query received`

The search backend rejected the query. On `/v1/emails` it means `query` was not a string. On the address search endpoints it means the query could not be executed. Simplify the query and retry.

### 40012 - Pagination Limit {#40012}

**HTTP 400.** Message: `It is not possible to paginate beyond the first 10,000 results. Please contact support if you need to extract an exhaustive list of addresses`

On `/v1/addresses` and `/v1/postcodes/:postcode`, `page` multiplied by `limit` reached 10,000. Narrow the query with filters, or contact support for a bulk extract.

### 40013 - Too Many Biases {#40013}

**HTTP 400.** Message: `Too many query biases requested. You may set up to 5 biases`

`/v1/autocomplete/addresses` accepts at most five bias parameters. Remove the extras.

### 40014 - Too Many Filters {#40014}

**HTTP 400.** Message: `Too many filters specified`

`/v1/autocomplete/addresses` accepts at most eight filter parameters. Remove the extras.

### 40016 - Invalid Filter or Bias {#40016}

**HTTP 400.** Message: `Invalid search query provided. Please review your inputs`

A filter or bias value could not be turned into a query, for example a malformed `bias_lonlat`. Check each filter and bias value against the [address search reference](https://docs.ideal-postcodes.co.uk/docs/api/find-address).

### 40017 - Email Query Too Long {#40017}

**HTTP 400.** Message: `Invalid email query string. Email string length is too long. Max length 320`

The `query` on `/v1/emails` is longer than 320 characters. Trim the input before sending it.

### 40018 - Missing Query {#40018}

**HTTP 400.** Message: `Invalid query. The q or query parameter is required`

`/v1/gbr/postcodes` received no `q` or `query` parameter, or `/v1/cleanse/addresses` received a `query` that is not a string. Send the query as a string.

## 401 Unauthorized

Codes 4010 and 4011 are covered under [Common fixes](#common-fixes).

### 4012 - Key Not Owned {#4012}

**HTTP 401.** Message: `Forbidden`

`GET /v1/keys/:key` received a `user_token` that does not own the key. Check both values against your dashboard.

### 4013 - Sub-licensee Key Required {#4013}

**HTTP 401.** Message: `A Sub Licensee Key is required to perform this action`

The `/v1/keys/:key/licensees` endpoints were called on a key without sub-licensing, or a sub-licensed key made a lookup without a `licensee` parameter. See the [sub-licensing guide](https://docs.ideal-postcodes.co.uk/docs/guides/sublicensing).

### 4014 - Licensee Belongs to Another Key {#4014}

**HTTP 401.** Message: `Invalid API Key provided for licensee`

The `licensee` parameter names a licensee created under a different key. Send the licensee with its parent key.

### 4015 - Not Licensed for This Data {#4015}

**HTTP 401.** Message: `Inadequate licence to access data. Your API Key is not licensed to access the data you attempted to query`

Your key has no dataset or service enabled for this endpoint. Raised by `/v1/addresses` and `/v1/postcodes/:postcode` when no address dataset is enabled, `/v1/emails` when email validation is off, `/v1/phone_numbers` when phone validation is off and `/v1/cleanse/addresses` when cleanse is off. Enable the dataset or service in your key settings, or contact support if it is not available on your account.

### 4016 - Invalid Context {#4016}

**HTTP 401.** Message: `You have requested an invalid context`

`/v1/cleanse/addresses` received a `context` the API does not support. Check the [address cleanse reference](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse) for the accepted values.

## 402 Payment Required

Codes 4020 and 4021 are covered under [Common fixes](#common-fixes).

### 404 - Page Not Found {#404}

**HTTP 404.** Message: `404 Page not found`

No route matched the request. The code is `404`, not `4040`. Check the path and the HTTP method against the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/api-reference).

### 4040 - Postcode Not Found {#4040}

**HTTP 404.** Message: `Postcode not found`

`/v1/postcodes/:postcode` found no match. The response adds `suggestions`, an array of nearby valid postcodes: the auto-corrected postcode if one exists, otherwise the closest matches. `/v1/gbr/postcodes/:postcode` and `/v1/gbr/outcodes/:outcode` return the same code without suggestions.

Offer the suggestions to the user, or fall back to an [address search](https://docs.ideal-postcodes.co.uk/docs/api/find-address).

### 4042 - Key Not Found {#4042}

**HTTP 404.** Message: `Key not found`

The key in a `/v1/keys/:key` path, or the licensee key it names, does not exist or has been deleted. Check the key against your dashboard.

### 4044 - UDPRN Not Found {#4044}

**HTTP 404.** Message: `No UDPRN found`

`/v1/addresses/:udprn` and `/v1/udprn/:udprn` found no address for the UDPRN. Values over 2,000,000,000 also raise this. Test with UDPRN `-1`.

### 4045 - Licensee Not Found {#4045}

**HTTP 404.** Message: `No licensee found`

The `licensee` parameter, or the licensee id in a `/v1/keys/:key/licensees/:licensee` path, does not exist under this key. List licensees with `GET /v1/keys/:key/licensees`.

### 4046 - UMPRN Not Found {#4046}

**HTTP 404.** Message: `No UMPRN found`

`/v1/umprn/:umprn` found no address for the UMPRN, the value is over 2,000,000,000, or your key does not have the Multiple Residence dataset enabled. Test with UMPRN `-1`.

### 4047 - Config Not Found {#4047}

**HTTP 404.** Message: `Config not found`

No config with that name exists under `/v1/keys/:key/configs/:config`. List configs with `GET /v1/keys/:key/configs`.

### 4048 - Address Not Found {#4048}

**HTTP 404.** Message: `Address not found`

`/v1/autocomplete/addresses/:id/gbr` or `/v1/places/:id` found nothing for the id. Ids come from a preceding autocomplete or places search. Check the id was copied in full.

### 4100 - Signup Link Expired {#4100}

**HTTP 410.** Message: `Signup link has expired or has already been claimed. Please re-run signup`

The CLI signup link at `/v1/sign_up/:cli_token` has expired or was already used. Run the signup again to get a fresh link.

### 4150 - Unsupported Media Type {#4150}

**HTTP 415.** Message: `Unsupported Media Type. Our POST, PATCH and PUT endpoints only support application/json`

A POST, PUT or PATCH request arrived without `Content-Type: application/json`. Set the header and send a JSON body.

### 4290 - Request Timed Out {#4290}

**HTTP 429.** Message: `Request timed out. Please wait and try again later`

`/v1/cleanse/addresses` or `/v1/phone_numbers` waited too long for an upstream response. Retry after a short delay.

### 4291 - Too Many Requests {#4291}

**HTTP 429.** Message: `Too many requests. Please contact support`

The request was flagged as high risk and the key is less than two days old. This protects new accounts from abuse. Contact support if it blocks a legitimate integration.

### 5001 - Uncatalogued Error {#5001}

**HTTP 500.** Message: `Uncatalogued Error`

An error the API does not recognise. Retry once. If it persists, contact support with the request and the time it was made.

### 5002 - Internal Timeout {#5002}

**HTTP 500.** Message: `Search request reached internal timeout limits`

A database query exceeded its time limit. Retry after a short delay. If it persists, contact support.

## Related guides

- [API Key Settings](https://docs.ideal-postcodes.co.uk/docs/guides/api-key-settings): daily, monthly and individual lookup limits behind 4021
- [Allowed URLs](https://docs.ideal-postcodes.co.uk/docs/guides/allowed-urls): the matching rules behind 4011
- [Automated Top-Ups](https://docs.ideal-postcodes.co.uk/docs/guides/automated-topups): stop 4020 recurring
- [Sublicensing](https://docs.ideal-postcodes.co.uk/docs/guides/sublicensing): licensee keys behind 4013, 4014 and 4045

[Contact us](https://ideal-postcodes.co.uk/support) with the request and the time it was made if an error is not listed here.
