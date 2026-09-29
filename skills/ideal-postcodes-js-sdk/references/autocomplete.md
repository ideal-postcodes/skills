# Autocomplete

The autocomplete helper turns a partial UK address into suggestions as the user types, then turns the chosen suggestion into a full address. It debounces keystrokes, drops stale requests and caches recent suggestions.

```ts title="autocomplete.ts"
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { createAutocomplete } from "@ideal-postcodes/sdk/autocomplete";

const client = createIdpcClient({ apiKey: "ak_test" });
const autocomplete = createAutocomplete({ client });

const suggest = async (query: string) => {
  const hits = await autocomplete.find({ query });
  if (!hits) return; // superseded by a newer find, or cancelled
  const hit = hits[0];
  if (!hit) return; // no matches
  const address = await autocomplete.resolve({ hit });
  console.log(address.line_1);
};

await suggest("10 Downing Street");

// Call when the input is removed or the component unmounts.
autocomplete.cancel();
```

## How do I find suggestions?

Call `find` with the partial address as `query`. It waits for typing to pause, then resolves to an array of hits. An empty array means nothing matched.

`find` also takes any other [`findAddress`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/find-address) parameter in the same object, such as `limit`, `context` or a filter. See [filter and bias](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/filter-and-bias) to restrict or rank suggestions by postcode, post town or location.

A newer `find` resolves the previous pending call with `undefined` and aborts its request. `cancel()` does the same without starting another. Superseded calls never reject, so check for `undefined` and return early.

## How do I let users switch country?

Load the countries your key can search with [`keyAvailability`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/key-availability), show them in a picker, then pass the chosen code as `context`. A key can be licensed for several countries, such as the UK (`GBR`) and Ireland (`IRL`). The check needs only the API key, so it can run in the browser.

```ts title="country-switch.ts"
import { createIdpcClient, keyAvailability } from "@ideal-postcodes/sdk";
import { createAutocomplete } from "@ideal-postcodes/sdk/autocomplete";

const apiKey = "ak_test";
const client = createIdpcClient({ apiKey });
const autocomplete = createAutocomplete({ client });

const { data } = await keyAvailability({ client, path: { key: apiKey } });
const { contexts, context } = data.result;
for (const country of contexts) console.log(country.emoji, country.description, country.iso_3);

// Start with the country that matches the user's IP address, if any.
let country = context || contexts[0]?.iso_3;
await autocomplete.find({ query: "10 Downing Street", context: country });

// When the user picks another country, search again with its code.
country = "IRL";
await autocomplete.find({ query: "1 Main Street, Dublin", context: country });
```

Each entry in `contexts` has `iso_3` (the `context` code), `iso_2`, `description` and `emoji`. `context` is the licensed country that best matches the caller's IP address, or an empty string when none does.

## How do I get the full address for a suggestion?

Pass the hit to `resolve`. It requests the full address from [`resolveAddress`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints/resolve-address) at once, without a debounce.

`resolve` returns the flat `Address` model. Use `native` for dataset-specific fields and narrow the union before accessing them. See the [API reference](https://openapi.ideal-postcodes.co.uk) for field definitions.

## What does autocomplete cost?

Suggestions are free. Each `resolve` costs one lookup. The API rate limits, then suspends, integrations that request suggestions without resolving any. The default limit is 3,000 suggestion requests per 5 minutes for each key and IP address.

## Which options can I set?

| Option | Default | Effect |
| --- | --- | --- |
| `debounceMs` | `100` | Milliseconds of quiet before `find` sends a request |
| `cache` | `{ max: 50, ttlMs: 60_000 }` | Size and lifetime of the suggestions cache. `false` disables it |

The cache belongs to one helper instance. Create a new client and helper when credentials change.

## Which types does it export?

`@ideal-postcodes/sdk/autocomplete` exports `CreateAutocompleteOptions`, `Autocomplete`, `Hit`, `ResolvedAddress`, `FindInput` and `ResolveInput` for typing your own wrappers.

## How are errors reported?

HTTP errors reject with [`ApiError`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors). Network failures reject with the runtime's native error. The helper does not cache failed requests. Pass `signal` to `find` or `resolve` to abort a request yourself, and the call rejects with `AbortError`.

React and Preact apps can use [TanStack Query](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/react#how-do-i-use-tanstack-query) for query caching instead.

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
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { createAutocomplete, type Autocomplete } from "@ideal-postcodes/sdk/autocomplete";

const form = document.querySelector<HTMLFormElement>("#credentials")!;
const key = document.querySelector<HTMLInputElement>("#api-key")!;
const output = document.querySelector<HTMLElement>("#output")!;

let autocomplete: Autocomplete | undefined;
form.addEventListener("submit", (event) => {
  event.preventDefault();
  autocomplete?.cancel();
  const current = createAutocomplete({ client: createIdpcClient({ apiKey: key.value }), debounceMs: 150 });
  autocomplete = current;
  const app = document.querySelector<HTMLElement>("#app")!;
  app.replaceChildren();
  const input = document.createElement("input");
  input.placeholder = "10 Downing Street";
  input.setAttribute("aria-label", "Address");
  const list = document.createElement("ul");
  app.append(input, list);
  input.addEventListener("input", async () => {
    list.replaceChildren();
    if (input.value.length < 3) { current.cancel(); return; }
    try {
      const hits = await current.find({ query: input.value });
      if (!hits) return;
      for (const hit of hits) {
        const item = document.createElement("li");
        const button = document.createElement("button");
        button.textContent = hit.suggestion;
        button.addEventListener("click", async () => {
          try {
            const address = await current.resolve({ hit });
            output.textContent = JSON.stringify(address, null, 2);
          } catch (error) { output.textContent = "Request failed: " + String(error); }
        });
        item.append(button);
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
