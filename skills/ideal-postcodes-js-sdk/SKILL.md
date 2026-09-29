---
name: ideal-postcodes-js-sdk
description: Use when integrating the Ideal Postcodes fetch-based TypeScript SDK. Covers API calls, autocomplete, errors and React or Preact Query adapters.
license: SEE LICENSE IN LICENSE
metadata:
  author: ideal-postcodes
  homepage: https://ideal-postcodes.co.uk
references:
  - setup.md
  - autocomplete.md
  - filter-and-bias.md
  - postcode-lookup.md
  - uprn-lookup.md
  - address-cleanse.md
  - react.md
  - errors.md
  - common.md
  - endpoints/
---

# Ideal Postcodes TypeScript SDK

Call the Ideal Postcodes API with typed functions over native fetch. Supports browsers and Node.js 22 or later.

```bash
npm install @ideal-postcodes/sdk
```

```ts
import { createIdpcClient, findAddress } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await findAddress({
  client,
  query: { query: "10 Downing Street" },
});
console.log(data.result.hits);
```

Pass the client to each operation. Each instance holds its own configuration. ESM bundlers can remove unused operations; autocomplete and framework adapters use separate imports.

## Authentication and errors

Set `apiKey` on the client. Private account operations also need a Management Key, set as `userToken`. Never put a Management Key in browser code. HTTP errors throw an `ApiError` with the HTTP `status` and the API error `code` and `message`. Pass `throwOnError: false` to get `{ data, error }` instead. See [error handling](./references/errors.md).

## References

- [Setup](./references/setup.md)
- [Autocomplete](./references/autocomplete.md)
- [Filter and bias](./references/filter-and-bias.md)
- [Postcode lookup](./references/postcode-lookup.md)
- [UPRN lookup](./references/uprn-lookup.md)
- [Address cleanse](./references/address-cleanse.md)
- [React and Preact](./references/react.md)
- [Error handling](./references/errors.md)
- [Common behaviour](./references/common.md)
- [Operations](./references/endpoints/)
