# `keyLogs`

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

This operation needs a Management Key. Set it as `userToken` on the client, which sends it in the `Authorization` header. See [setup](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/setup).

## Import

```ts
import { keyLogs } from "@ideal-postcodes/sdk";
```

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `string` | yes | **API Key** The API Key to retrieve. Begins `ak_`. |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `start` | `number` | no | **Start Timestamp** A start date/time in the form of a UNIX Timestamp in milliseconds. E.g. `1418556452651` |
| `end` | `number` | no | **End Timestamp** An end date/time in the form of a UNIX Timestamp in milliseconds. E.g. `1418556477882` |
| `licensee` | `string` | no | **Licensee Key** Uniquely identifies a licensee. |

## Response Type

```ts
import type { KeyLogsResponse } from "@ideal-postcodes/sdk";
```

## Errors

HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Network failures and aborts reject with the runtime's native error. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## Example

```ts title="key-logs.ts"
import { createIdpcClient, keyLogs } from "@ideal-postcodes/sdk";

export const example = async () => {
  const client = createIdpcClient({
    apiKey: "ak_test",
    userToken: "uk_example",
  });
  const { data } = await keyLogs({
    client,
    path: { key: "ak_test" },
  });
  return data;
};
```
