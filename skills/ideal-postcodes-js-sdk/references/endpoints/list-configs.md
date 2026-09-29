# `listConfigs`

List

Returns every configuration stored against a key, oldest first.

## Endpoint

`GET /keys/{key}/configs`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/list-configs) for this endpoint.

This operation needs a Management Key. Set it as `userToken` on the client, which sends it in the `Authorization` header. See [setup](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/setup).

## Import

```ts
import { listConfigs } from "@ideal-postcodes/sdk";
```

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `string` | yes | **API Key** The API Key to retrieve. Begins `ak_`. |

## Query Parameters

_None._

## Response Type

```ts
import type { ListConfigsResponse } from "@ideal-postcodes/sdk";
```

## Errors

HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Network failures and aborts reject with the runtime's native error. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## Example

```ts title="list-configs.ts"
import { createIdpcClient, listConfigs } from "@ideal-postcodes/sdk";

export const example = async () => {
  const client = createIdpcClient({
    apiKey: "ak_test",
    userToken: "uk_example",
  });
  const { data } = await listConfigs({
    client,
    path: { key: "ak_test" },
  });
  return data;
};
```
