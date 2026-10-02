# `resolveAddress`

Resolve Address

Returns the complete address for an autocomplete suggestion, identified by its address ID.

This is the step of the autocomplete flow that costs a lookup. Fetching suggestions is free.

The API returns resolved addresses, including addresses outside the UK, in a UK format (up to 3 address lines) using UK nomenclature such as postcode and county.

An ID that matches no address returns `404`.

## Endpoint

`GET /autocomplete/addresses/{address}/gbr`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/resolve-address) for this endpoint.

## Import

```ts
import { resolveAddress } from "@ideal-postcodes/sdk";
```

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `address` | `string` | yes | **ID of address suggestion** ID of address suggestion provided by the API to fully resolve. |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `api_key` | `string` | no | **API Key** Your unique identifier that allows access to our APIs. Begins `ak_`. Available from your dashboard. |
| `tags` | `string` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |

## Response Type

```ts
import type { ResolveAddressResponse } from "@ideal-postcodes/sdk";
```

## Errors

HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Network failures and aborts reject with the runtime's native error. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## Example

```ts title="resolve-address.ts"
import { createIdpcClient, resolveAddress } from "@ideal-postcodes/sdk";

export const example = async () => {
  const client = createIdpcClient({ apiKey: "ak_test" });
  const { data } = await resolveAddress({
    client,
    path: { address: "paf_2670849" },
  });
  return data;
};
```
