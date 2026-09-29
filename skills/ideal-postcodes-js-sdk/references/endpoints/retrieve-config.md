# `retrieveConfig`

Retrieve

Returns a configuration by name. This request needs no `user_token`, so a browser integration can read its own configuration at runtime.

## Endpoint

`GET /keys/{key}/configs/{config}`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/retrieve-config) for this endpoint.

## Import

```ts
import { retrieveConfig } from "@ideal-postcodes/sdk";
```

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `string` | yes | **API Key** The API Key to retrieve. Begins `ak_`. |
| `config` | `string` | yes | **Configuration Name** User-provided configuration object name. |

## Query Parameters

_None._

## Response Type

```ts
import type { RetrieveConfigResponse } from "@ideal-postcodes/sdk";
```

## Errors

HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Network failures and aborts reject with the runtime's native error. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## Example

```ts title="retrieve-config.ts"
import { createIdpcClient, retrieveConfig } from "@ideal-postcodes/sdk";

export const example = async () => {
  const client = createIdpcClient({ apiKey: "ak_test" });
  const { data } = await retrieveConfig({
    client,
    path: { key: "ak_test", config: "checkout" },
  });
  return data;
};
```
