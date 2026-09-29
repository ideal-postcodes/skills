# React and Preact

Use the SDK to build your own UK address search component in React or Preact. The examples below search as the user types.

## Should I use the SDK or a ready-made component?

To add address search to a form, use [Address Finder for React](https://docs.ideal-postcodes.co.uk/docs/address-finder/react) or [Postcode Lookup for React](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/react). They provide the interface and fill in your address fields. Use the SDK when you need your own interface.

## How do I build a search component without dependencies?

Use the [autocomplete helper](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/autocomplete) in a hook. It debounces keystrokes and drops stale requests, so you need no data-fetching library.

```bash
npm install @ideal-postcodes/sdk
```

#### React

```tsx title="Search.tsx"
import { useEffect, useState } from "react";
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { createAutocomplete, type Hit } from "@ideal-postcodes/sdk/autocomplete";

const autocomplete = createAutocomplete({
  client: createIdpcClient({ apiKey: "ak_test" }),
});

export function Search() {
  const [query, setQuery] = useState("");
  const [hits, setHits] = useState<Hit[]>([]);

  useEffect(() => {
    if (query.length < 3) return setHits([]);
    autocomplete
      .find({ query })
      .then((found) => found && setHits(found))
      .catch(console.error);
    return () => autocomplete.cancel();
  }, [query]);

  return (
    <div>
      <input value={query} onInput={(event) => setQuery(event.currentTarget.value)} aria-label="Address" />
      <ul>{hits.map((hit) => <li key={hit.id}>{hit.suggestion}</li>)}</ul>
    </div>
  );
}
```

#### Preact

```tsx title="Search.tsx"
import { useEffect, useState } from "preact/hooks";
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { createAutocomplete, type Hit } from "@ideal-postcodes/sdk/autocomplete";

const autocomplete = createAutocomplete({
  client: createIdpcClient({ apiKey: "ak_test" }),
});

export function Search() {
  const [query, setQuery] = useState("");
  const [hits, setHits] = useState<Hit[]>([]);

  useEffect(() => {
    if (query.length < 3) return setHits([]);
    autocomplete
      .find({ query })
      .then((found) => found && setHits(found))
      .catch(console.error);
    return () => autocomplete.cancel();
  }, [query]);

  return (
    <div>
      <input value={query} onInput={(event) => setQuery(event.currentTarget.value)} aria-label="Address" />
      <ul>{hits.map((hit) => <li key={hit.id}>{hit.suggestion}</li>)}</ul>
    </div>
  );
}
```

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

```tsx
import React from "react";
import { createRoot } from "react-dom/client";
import { useEffect, useState } from "react";
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { createAutocomplete, type Autocomplete, type Hit } from "@ideal-postcodes/sdk/autocomplete";

let autocomplete: Autocomplete;

function Search() {
  const [query, setQuery] = useState("");
  const [hits, setHits] = useState<Hit[]>([]);

  useEffect(() => {
    if (query.length < 3) return setHits([]);
    autocomplete
      .find({ query })
      .then((found) => found && setHits(found))
      .catch((error) => (output.textContent = "Request failed: " + String(error)));
    return () => autocomplete.cancel();
  }, [query]);

  return (
    <div>
      <input value={query} placeholder="10 Downing Street" onInput={(event) => setQuery(event.currentTarget.value)} aria-label="Address" />
      <ul>{hits.map((hit) => <li key={hit.id}>{hit.suggestion}</li>)}</ul>
    </div>
  );
}

const form = document.querySelector<HTMLFormElement>("#credentials")!;
const apiKey = document.querySelector<HTMLInputElement>("#api-key")!;
const output = document.querySelector<HTMLElement>("#output")!;

const root = createRoot(document.querySelector("#app")!);
form.addEventListener("submit", (event) => {
  event.preventDefault();
  autocomplete?.cancel();
  autocomplete = createAutocomplete({ client: createIdpcClient({ apiKey: apiKey.value }) });
  root.render(<Search key={apiKey.value} />);
});
form.querySelector<HTMLButtonElement>("button")!.disabled = false;
output.textContent = "";
```

## How do I use TanStack Query?

Import the query options for the operation you need. The SDK exports TanStack Query options, query keys and mutations for every operation from `@ideal-postcodes/sdk/react-query` and `@ideal-postcodes/sdk/preact-query`. Install the matching peer, `@tanstack/react-query` or `@tanstack/preact-query`. Wrap the component in a `QueryClientProvider`.

```bash
npm install @ideal-postcodes/sdk @tanstack/react-query
```

#### React

```tsx title="App.tsx"
import { useState } from "react";
import { QueryClient, QueryClientProvider, useQuery } from "@tanstack/react-query";
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { findAddressOptions } from "@ideal-postcodes/sdk/react-query";

const client = createIdpcClient({ apiKey: "ak_test" });
const queryClient = new QueryClient();

function Search() {
  const [query, setQuery] = useState("");
  const result = useQuery({
    ...findAddressOptions({ client, query: { query } }),
    enabled: query.length >= 3,
    staleTime: 60_000,
  });
  return (
    <div>
      <input value={query} onInput={(event) => setQuery(event.currentTarget.value)} aria-label="Address" />
      {result.isError && <p>Address search failed.</p>}
      <ul>{result.data?.result.hits.map((hit) => <li key={hit.id}>{hit.suggestion}</li>)}</ul>
    </div>
  );
}

export function App() {
  return <QueryClientProvider client={queryClient}><Search /></QueryClientProvider>;
}
```

#### Preact

```tsx title="App.tsx"
import { useState } from "preact/hooks";
import { QueryClient, QueryClientProvider, useQuery } from "@tanstack/preact-query";
import { createIdpcClient } from "@ideal-postcodes/sdk";
import { findAddressOptions } from "@ideal-postcodes/sdk/preact-query";

const client = createIdpcClient({ apiKey: "ak_test" });
const queryClient = new QueryClient();

function Search() {
  const [query, setQuery] = useState("");
  const result = useQuery({
    ...findAddressOptions({ client, query: { query } }),
    enabled: query.length >= 3,
    staleTime: 60_000,
  });
  return (
    <div>
      <input value={query} onInput={(event) => setQuery(event.currentTarget.value)} aria-label="Address" />
      {result.isError && <p>Address search failed.</p>}
      <ul>{result.data?.result.hits.map((hit) => <li key={hit.id}>{hit.suggestion}</li>)}</ul>
    </div>
  );
}

export function App() {
  return <QueryClientProvider client={queryClient}><Search /></QueryClientProvider>;
}
```

Pass the client to each builder. Queries forward TanStack Query's cancellation signal and throw on failure, so a failed query's `error` is the `ApiError`, or the native error for a network failure. Debounce the query value before passing it to `findAddressOptions` if you want fewer requests.

Use separate query caches when switching credentials. Do not share an authenticated query cache between server requests or users.
