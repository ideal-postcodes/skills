# `postcodes`

Lookup Postcode

Returns the complete list of addresses for a postcode. Postcode searches are space and case insensitive.

Each request looks up one postcode. To extract the addresses for several postcodes, send one request per postcode.

Use it to power postcode driven address searches, like [Postcode Lookup](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/).

Postcode lookup covers the United Kingdom, the Republic of Ireland, the Netherlands and Singapore. The API detects the format of the postcode you submit. UK and Irish postcodes are searched by default. For a Dutch or Singapore postcode, set `context` to `NLD` or `SGP`, or to `GLOBAL` to accept any supported format.

UK postcodes need PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium on your key. Eircodes need ECAD or ECAF. Dutch postcodes need Kadaster. Singapore postcodes need HERE Asia Pacific. Without a matching licence the request is rejected.

An unfound postcode costs no lookup. A postcode that returns addresses costs one.

## Postcode Not Found

Invalid postcodes do not affect your lookup balance. The API returns a `404` response with this body:

```json
{
"code": 4040,
"message": "Postcode not found",
"suggestions": ["SW1A 0AA"]
}
```

### Suggestions

If a postcode cannot be found, the API returns up to 5 of the closest matching postcodes. It corrects common errors first (e.g. mixing up `O` and `0` or `I` and `1`).

If the suggestion list is small (fewer than 3), the correct postcode is likely to be among them. Notify the user or trigger new searches immediately.

The suggestion list is empty if the postcode has deviated too far from a valid postcode format.

## Multiple Residence

A small number of postcodes return more than 100 premises. The API returns 100 addresses per page, so use `page` to paginate the result set.

## Endpoint

`GET /postcodes/{postcode}`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/postcodes) for this endpoint.

## Import

```ts
import { postcodes } from "@ideal-postcodes/sdk";
```

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `postcode` | `string` | yes | Postcode to retrieve |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `api_key` | `string` | no | **API Key** Your unique identifier that allows access to our APIs. Begins `ak_`. Available from your dashboard. |
| `filter` | `string` | no | **Restrict Result Fields** Comma separated whitelist of address elements to return. E.g. `filter=line_1,line_2,line_3` returns only the `line_1`, `line_2` and `line_3` address elements in your response. |
| `page` | `number` | no | **Page** 0 indexed indicator of the page of results to receive. Virtually all postcode results are returned on page 0. A small number of Multiple Residence postcodes may need pagination (i.e. have more than 100 premises). |
| `tags` | `string` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |
| `dataset` | `Array<Dataset>` | no | **Filter by Dataset** Comma-separated list of datasets to search within. Filters results to only include addresses from the specified datasets. Useful for keys with multiple overlapping datasets enabled (e.g. `paf` and `abp`). |
| `context` | `string` | no | **Context** Limits search results, typically within a country. |

## Response Type

```ts
import type { PostcodesResponse } from "@ideal-postcodes/sdk";
```

## Errors

HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Network failures and aborts reject with the runtime's native error. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## Example

```ts title="postcodes.ts"
import { createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

export const example = async () => {
  const client = createIdpcClient({ apiKey: "ak_test" });
  const { data } = await postcodes({
    client,
    path: { postcode: "SW1A 2AA" },
  });
  return data;
};
```
