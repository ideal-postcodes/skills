# Migrate from the core clients

`@ideal-postcodes/core-node`, `@ideal-postcodes/core-axios`, `@ideal-postcodes/core-browser` and `@ideal-postcodes/core-interface` are deprecated. They receive no new features. Use [`@ideal-postcodes/sdk`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript) instead. It runs on native `fetch` in browsers and Node.js 22 or later, with no runtime dependencies.

If you use Address Finder or Postcode Lookup, you need do nothing. The widgets manage their own API client.

```bash
npm uninstall @ideal-postcodes/core-axios
npm install @ideal-postcodes/sdk
```

## How do I look up a postcode?

```ts title="before.ts"
import { Client, lookupPostcode } from "@ideal-postcodes/core-axios";

const client = new Client({ api_key: "ak_test" });
const addresses = await lookupPostcode({ client, postcode: "BR8 7RE" });
```

```ts title="after.ts"
import { createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
const { data } = await postcodes({ client, path: { postcode: "BR8 7RE" } });
const addresses = data.result;
```

`lookupPostcode` returned `[]` for an unknown postcode. `postcodes` throws an `ApiError` with code `4040`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors).

## What replaces each function?

| core clients | `@ideal-postcodes/sdk` |
| --- | --- |
| `new Client({ api_key })` | `createIdpcClient({ apiKey })` |
| `lookupPostcode({ client, postcode })` | `postcodes({ client, path: { postcode } })`, then `data.result` |
| `lookupAddress({ client, query })` | `addresses({ client, query: { query } })`, then `data.result.hits` |
| `autocomplete.list` / `autocomplete.resolve` | `findAddress` / `resolveAddress`, or [`createAutocomplete`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/autocomplete) |
| `checkKeyUsability` / `keys.retrieve` | `keyAvailability({ client, path: { key } })` |
| `IdpcRequestFailedError` and other error classes | `ApiError`, with `status`, `code` and `message` |

`createIdpcClient` also takes `baseUrl`, `fetch`, `headers` and `timeoutMs`. Pass `throwOnError: false` to an operation to get `{ data, error }` instead of a rejection.

Every operation has its own [reference page](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints).
