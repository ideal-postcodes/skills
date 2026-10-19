# `key_logs`

Logs (CSV)

Returns a CSV of the paid lookups made on a key, with the information recorded against each one.

This method requires your Management Key, which can be found on your [accounts page](https://account.ideal-postcodes.co.uk/account).

You can request a maximum interval of 90 days. Without a start or end date, the interval defaults to the last 21 days.

The `Content-Type` returned is CSV (text/csv). For a non-200 response it reverts to JSON, with the error code and message in the body.

## CSV Format

The CSV has no header row. Columns, in order:

1. Timestamp (ISO 8601)
2. IP address the request was received from
3. Search term
4. URL the request originated from
5. Lookup type
6. Tags
7. Lookups consumed
8. Licensee name (sublicensing keys only)
9. Source IP address

The source IP column carries the address forwarded in the `IDPC-Source-IP` header. It is only recorded for keys with IP address forwarding enabled, and only when the header holds a valid IP address. It is empty otherwise.

## Data Redaction

We redact Personally Identifiable Data (PII) in your usage log (including IP, source IP, search term and URL data) weekly.

By default we redact PII older than 28 days. You can change this period from your dashboard.

Set the interval to `0` days to prevent PII collection altogether.

## Endpoint

`GET /keys/{key}/lookups`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/key-logs) for this endpoint.

This operation needs a Management Key. Pass it as the required `user_token` keyword argument; the client sends it in the `Authorization` header. See [setup](https://docs.ideal-postcodes.co.uk/docs/sdks/python/setup).

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `key_logs` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `str` | yes | **API Key** The API Key to retrieve. Begins `ak_`. |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `start` | `int` | no | **Start Timestamp** A start date/time in the form of a UNIX Timestamp in milliseconds. E.g. `1418556452651` |
| `end` | `int` | no | **End Timestamp** An end date/time in the form of a UNIX Timestamp in milliseconds. E.g. `1418556477882` |
| `licensee` | `str` | no | **Licensee Key** Uniquely identifies a licensee. |

## Response Type

Returns the CSV text as a `str`.

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="key_logs.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    csv = client.key_logs("ak_test", user_token="uk_example")
```
