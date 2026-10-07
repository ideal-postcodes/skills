# Postcode lookup

`lookupPostcode` returns every address for a UK postcode. Searches ignore case and spacing, so `br87re` finds `BR8 7RE`.

## How do I look up a postcode?

Pass the postcode. The addresses sit in `addresses`.

```ts title="postcode-lookup.ts"
import { createIdpcClient, lookupPostcode } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { addresses } = await lookupPostcode({ client, postcode: "BR8 7RE" });
console.log(addresses);
```

## What if the postcode does not exist?

`addresses` is empty and `suggestions` carries up to 5 of the closest matching postcodes. If there are fewer than 3, the right postcode is likely among them. An unknown postcode costs no lookup.

```ts
const { addresses, suggestions } = await lookupPostcode({
  client,
  postcode: "BR8 7RX",
});
if (addresses.length === 0) console.log("Did you mean", suggestions);
```

Other errors, such as an invalid key, throw an [`ApiError`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## What does a postcode lookup cost?

A postcode that returns addresses costs one lookup. Your key needs one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium for UK postcodes.

## What about postcodes with more than 100 addresses?

The API returns 100 addresses per page. `total` counts the addresses across every page. Pages start at `0`. Only a small number of postcodes need it.

```ts
const { addresses, total } = await lookupPostcode({ client, postcode });
for (let page = 1; page < Math.ceil(total / 100); page++) {
  const next = await lookupPostcode({ client, postcode, page });
  addresses.push(...next.addresses);
}
```

## How do I look up Irish, Dutch or Singapore postcodes?

Pass `context`, for example `context: "NLD"`. `lookupPostcode` takes the same query parameters as [`postcodes`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/postcodes). The [API reference](https://docs.ideal-postcodes.co.uk/docs/api/postcodes) documents the response.

## Try it

Select **Look up** to run the lookup. Edit the postcode in the code to try another.

```html
<button>Look up</button>
<pre></pre>
```

```javascript
  import { createIdpcClient, lookupPostcode } from "@ideal-postcodes/sdk";

  const apiKey = "ak_test"; // Replace with your own API key
  const client = createIdpcClient({ apiKey });

  document.querySelector("button").addEventListener("click", async () => {
    const result = await lookupPostcode({ client, postcode: "BR8 7RE" });
    document.querySelector("pre").textContent = JSON.stringify(result, null, 2);
  });
```

## Low-level access

`lookupPostcode` wraps `postcodes`, the generated endpoint function. Call `postcodes` directly for the full response. It throws an `ApiError` with status `404` and code `4040` on an unknown postcode, with the suggestions in the error body. See [result objects](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors#how-do-i-handle-errors-without-trycatch) to read them without a `try` block.

```ts
import { createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await postcodes({
  client,
  path: { postcode: "BR8 7RE" },
});
console.log(data.result);
```
