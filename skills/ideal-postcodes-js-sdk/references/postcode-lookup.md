# Postcode lookup

`postcodes` returns every address for a UK postcode. Searches ignore case and spacing, so `sw1a2aa` finds `SW1A 2AA`.

## How do I look up a postcode?

Pass the postcode as `path.postcode`. The addresses sit in `data.result`.

```ts title="postcode-lookup.ts"
import { createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await postcodes({
  client,
  path: { postcode: "SW1A 2AA" },
});
console.log(data.result);
```

## What if the postcode does not exist?

The call throws an [`ApiError`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors) with status `404` and code `4040`, and costs no lookup. The error body carries up to 5 `suggestions`, the closest matching postcodes. If there are fewer than 3, the right postcode is likely among them. See [result objects](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors#how-do-i-handle-errors-without-trycatch) to read them without a `try` block.

## What does a postcode lookup cost?

A postcode that returns addresses costs one lookup. Your key needs one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium for UK postcodes.

## What about postcodes with more than 100 addresses?

The API returns 100 addresses per page. Pass `query: { page: 1 }` for the next 100. Only a small number of postcodes need it.

See [`postcodes`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/postcodes) for Irish, Dutch and Singapore postcodes. The [API reference](https://docs.ideal-postcodes.co.uk/docs/api/postcodes) documents the response.

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
import { createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

const form = document.querySelector<HTMLFormElement>("#credentials")!;
const key = document.querySelector<HTMLInputElement>("#api-key")!;
const output = document.querySelector<HTMLElement>("#output")!;

form.addEventListener("submit", (event) => {
  event.preventDefault();
  const client = createIdpcClient({ apiKey: key.value });
  const search = document.createElement("form");
  const input = document.createElement("input");
  input.value = "SW1A 2AA";
  input.setAttribute("aria-label", "Postcode");
  const button = document.createElement("button");
  button.textContent = "Look up";
  const list = document.createElement("ul");
  search.append(input, button);
  document.querySelector<HTMLElement>("#app")!.replaceChildren(search, list);
  search.addEventListener("submit", async (event) => {
    event.preventDefault();
    list.replaceChildren();
    output.textContent = "";
    try {
      const { data } = await postcodes({ client, path: { postcode: input.value } });
      for (const address of data.result) {
        const item = document.createElement("li");
        item.textContent = [address.line_1, address.post_town, address.postcode].filter(Boolean).join(", ");
        list.append(item);
      }
    } catch (error) {
      output.textContent = "Request failed: " + String(error);
    }
  });
});
form.querySelector<HTMLButtonElement>("button")!.disabled = false;
output.textContent = "";
```
