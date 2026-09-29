# Address cleanse

`addressCleanse` matches a freeform UK address to the closest address on file. It returns the match, a confidence score and a match level for each address element.

## How do I clean an address?

Send the address as `body.query`. The match and its score sit in `data.result`.

```ts title="address-cleanse.ts"
import { createIdpcClient, addressCleanse } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await addressCleanse({
  client,
  body: { query: "10 Downing Street, London, SW1A 2AA" },
});
console.log(data.result.match, data.result.confidence);
```

## What does a cleanse cost?

A cleanse that returns a match costs one lookup. A no-match costs nothing.

## What confidence score should I accept?

No single threshold suits every project. Each incorrect, missing or misspelled element lowers the score, and inputs repeat the same errors within a project, so set the threshold per project. Sort a sample of results by score and note where matches turn ambiguous or wrong.

See [`addressCleanse`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/address-cleanse) for the request fields, and the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse) for the match levels.

## Try it

Enter a browser API key and select **Start example**. Requests use your account. Do not enter a Management Key.

```html
<form id="credentials">
  <label>API key <input id="api-key" type="password" required autocomplete="off" /></label>
  <button disabled>Start example</button>
</form>
<div id="app"></div>
<pre id="output" role="status">Loading example...</pre>
```

```ts
import { createIdpcClient, addressCleanse } from "@ideal-postcodes/sdk";

const form = document.querySelector<HTMLFormElement>("#credentials")!;
const key = document.querySelector<HTMLInputElement>("#api-key")!;
const output = document.querySelector<HTMLElement>("#output")!;

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const client = createIdpcClient({ apiKey: key.value });
  const search = document.createElement("form");
  const input = document.createElement("input");
  input.value = "10 Downing Street, London, SW1A 2AA";
  input.setAttribute("aria-label", "Address");
  const button = document.createElement("button");
  button.textContent = "Cleanse";
  const list = document.createElement("ul");
  search.append(input, button);
  document.querySelector<HTMLElement>("#app")!.replaceChildren(search, list);
  search.addEventListener("submit", async (event) => {
    event.preventDefault();
    list.replaceChildren();
    output.textContent = "";
    try {
      const { data } = await addressCleanse({ client, body: { query: input.value } });
      const { match, confidence } = data.result;
      output.textContent = match
        ? [match.line_1, match.post_town, match.postcode].filter(Boolean).join(", ") + " (confidence " + confidence + ")"
        : "No match";
    } catch (error) {
      output.textContent = "Request failed: " + String(error);
    }
  });
});
form.querySelector<HTMLButtonElement>("button")!.disabled = false;
output.textContent = "";
```
