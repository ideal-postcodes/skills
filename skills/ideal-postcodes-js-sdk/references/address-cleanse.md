# Address cleanse

`addressCleanse` matches a freeform UK address to the closest address on file. It returns the match, a confidence score and a match level for each address element.

## Clean an address

Send the address as `body.query`. The match and its score sit in `data.result`.

```ts title="address-cleanse.ts"
import { createIdpcClient, addressCleanse } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await addressCleanse({
  client,
  body: { query: "9 Malyons Road, Swanley, BR8 7RE" },
});
console.log(data.result.match, data.result.confidence);
```

## Cost

A cleanse that returns a match costs one lookup. A no-match costs nothing.

## Confidence threshold

No single threshold suits every project. Each incorrect, missing or misspelled element lowers the score, and inputs repeat the same errors within a project, so set the threshold per project. Sort a sample of results by score and note where matches turn ambiguous or wrong.

See [`addressCleanse`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/address-cleanse) for the request fields, and the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse) for the match levels.

## Try it

Select **Cleanse** to run the cleanse. Edit the address in the code to try another.

```html
<button>Cleanse</button>
<pre></pre>
```

```javascript
  import { createIdpcClient, addressCleanse } from "@ideal-postcodes/sdk";

  const apiKey = "ak_test"; // Replace with your own API key
  const client = createIdpcClient({ apiKey });

  document.querySelector("button").addEventListener("click", async () => {
    const { data } = await addressCleanse({
      client,
      body: { query: "9 Malyons Road, Swanley, BR8 7RE" },
    });
    document.querySelector("pre").textContent = JSON.stringify(data, null, 2);
  });
```
