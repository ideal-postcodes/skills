# UPRN lookup

Pass a UPRN to `addresses` as the `uprn` filter to retrieve the address it identifies. The UPRN (Unique Property Reference Number) is the number UK local authorities assign to each property and Ordnance Survey maintains.

## How do I look up an address by UPRN?

Call `addresses` with `uprn` and no `query`.

```ts title="uprn-lookup.ts"
import { createIdpcClient, addresses } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await addresses({
  client,
  query: { uprn: 100023336956 },
});
const [address] = data.result.hits;
console.log(address?.line_1, address?.post_town, address?.postcode);
```

The `uprn` filter takes one UPRN per request. It does not accept a comma separated list.

## What does the response look like?

The address sits in `result.hits`. This response is trimmed to the identifiers and postal fields:

```json
{
  "code": 2000,
  "message": "Success",
  "result": {
    "hits": [
      {
        "dataset": "paf",
        "uprn": "100023336956",
        "udprn": 23747771,
        "line_1": "Prime Minister & First Lord Of The Treasury",
        "line_2": "10 Downing Street",
        "post_town": "London",
        "postcode": "SW1A 2AA",
        "country": "England"
      }
    ]
  }
}
```

`dataset` names the source of the address, such as `paf` for Royal Mail's Postcode Address File. `udprn` is Royal Mail's delivery point reference for the same premise.

The API returns `uprn` as a string. A UPRN runs to 12 digits, so store it as text or as a 64-bit integer. Spreadsheet applications such as Excel corrupt values of that length.

## Which addresses carry a UPRN?

UK addresses carry a UPRN, taken from Ordnance Survey AddressBase. A small number of Great Britain addresses return an empty string while AddressBase catches up, as do addresses outside the UK. Multiple Residence premises share the UPRN of their parent premise.

## What does a UPRN lookup cost?

A request that returns an address costs one lookup. An empty result costs nothing. Your key needs at least one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium, or the API rejects the request.

See [`addresses`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/addresses) for every parameter and the response type.
