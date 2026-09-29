# Filter and bias

Filters restrict address suggestions to matching addresses. Biases rank matching addresses first but still return the rest. Both are query parameters on [`findAddress`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/find-address), and the [autocomplete helper](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/autocomplete) accepts them alongside `query` in `find`.

## How do I restrict suggestions to one area?

Add a filter, such as `postcode_outward`, to the query.

```ts title="filter.ts"
import { createIdpcClient, findAddress } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await findAddress({
  client,
  query: { query: "downing street", postcode_outward: "SW1A" },
});
console.log(data.result.hits);
```

With the autocomplete helper, pass the same parameter to `find`:

```ts title="filter-autocomplete.ts"
const hits = await autocomplete.find({ query: "high street", country: "Wales" });
```

An invalid filter term returns an empty list.

## How do I rank addresses in an area first?

Add a bias, such as `bias_post_town`. Addresses outside the post town still appear, ranked lower.

```ts title="bias.ts"
const { data } = await findAddress({
  client,
  query: { query: "high street", bias_post_town: "Edinburgh" },
});
```

To favour addresses near a point, pass `bias_lonlat` as longitude, latitude and radius in metres. To favour addresses near the user, set `bias_ip` to `"true"`. The API uses the approximate location of the IP address that sends the request, so call it from the browser rather than your server.

```ts title="bias-location.ts"
const near = await autocomplete.find({
  query: "high street",
  bias_lonlat: "-0.12767,51.503541,5000",
});
```

An invalid bias term has no effect.

## Which filters can I use?

| Filter | Restricts suggestions to | Example |
| --- | --- | --- |
| `postcode` | A full postcode | `SW1A 2AA` |
| `postcode_outward` | An outward code, the first half of a postcode | `SW1A` |
| `postcode_area` | A postcode area, the leading letters of a postcode | `SW` |
| `postcode_sector` | A postcode sector, the outward code plus the first digit of the inward code | `SW1A 2` |
| `post_town` | A Royal Mail post town, city or locality | `London` |
| `uprn` | One UPRN. Takes a number and accepts a single term only | `100023336956` |
| `country` | A country: `England`, `Scotland`, `Wales`, `Northern Ireland`, `Jersey`, `Guernsey` or `Isle of Man` | `Scotland` |
| `postcode_type` | Royal Mail postcode user type: `S` small user, `L` large user | `L` |

`country` never takes `United Kingdom`. Pass one of the seven values above.

A small user postcode covers a group of delivery points, 19 on average and never more than 100. A large user postcode belongs to one address, because of the volume of mail it receives or because it has a PO Box or Selectapost service.

## Which biases can I use?

| Bias | Ranks first | Example |
| --- | --- | --- |
| `bias_postcode` | A full postcode | `SW1A 2AA` |
| `bias_postcode_outward` | An outward code | `SW1A` |
| `bias_post_town` | A post town, city or locality | `Manchester` |
| `bias_lonlat` | A circle given as `longitude,latitude,radius`, radius in metres up to `50000` | `-2.095,57.15,100` |
| `bias_ip` | The approximate location of the requesting IP address. Takes `"true"` only | `"true"` |

## Can I pass several terms?

Yes. Separate terms with commas to match any of them, such as `postcode_area: "SW,SE"`. Separate filters combine, so each one narrows the list further.

The API accepts at most 8 filter terms and 5 bias terms per request. `uprn` takes one term, and `bias_lonlat` takes one circle.

See [`findAddress`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/find-address) for every parameter and the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/find-address) for the underlying request.
