# Setup

Install `@ideal-postcodes/sdk`, then create a client with your API key. The SDK runs in browsers and Node.js 22 or later and uses the runtime's native `fetch`.

## How do I create a client?

Install the package, then pass your API key to `createIdpcClient`.

```bash
npm install @ideal-postcodes/sdk
```

```ts title="client.ts"
import { createIdpcClient } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
```

## Which client options are there?

`createIdpcClient` takes one required option and five optional ones.

| Option | Type | Purpose |
| --- | --- | --- |
| `apiKey` | `string` | Required API key, sent as `api_key` |
| `userToken` | `string` | Management Key for account operations. Keep it on your server |
| `baseUrl` | `string` | Defaults to `https://api.ideal-postcodes.co.uk/v1` |
| `fetch` | `typeof fetch` | Custom fetch implementation for tests or request handling |
| `headers` | `HeadersInit` | Headers sent with every request, such as `User-Agent`. Per-call `headers` take precedence |
| `timeoutMs` | `number` | Milliseconds before each request rejects with `TimeoutError`. No timeout by default |

## How do I set a request timeout?

Set `timeoutMs` on the client. A request that runs longer rejects with a `DOMException` named `TimeoutError`, as described in [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors#how-do-i-tell-a-timeout-from-a-cancellation). A per-call `signal` can still cancel the request sooner.

```ts title="timeout.ts"
const client = createIdpcClient({ apiKey: "ak_test", timeoutMs: 30_000 });
```

To time out a single call, pass `signal: AbortSignal.timeout(5_000)` to the operation instead.

## How do I send default headers?

Pass `headers` to the client to send them with every request, for example a `User-Agent` that identifies your app. Headers passed to an operation take precedence. Browsers may drop a custom `User-Agent`, so set it on server-side clients.

```ts title="headers.ts"
const client = createIdpcClient({
  apiKey: "ak_test",
  headers: { "User-Agent": "my-app/1.0" },
});
```

## When do I need a Management Key?

Set `userToken` to a Management Key only to call the account operations that need it: key details, usage and logs, licensees and configs. Each [operation page](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/endpoints) says whether it needs one. A Management Key controls your account, so keep it on your server and never use it in a client that runs in the browser. The SDK sends it in the `Authorization` header, and only to operations that accept it.

## Does the SDK need other packages?

No. The root package has no runtime dependencies. Install `@tanstack/react-query` or `@tanstack/preact-query` only when using the matching [adapter](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/react#how-do-i-use-tanstack-query).

## Does it support CommonJS?

Yes. CommonJS consumers can use `require("@ideal-postcodes/sdk")`, though only ESM imports tree-shake. TypeScript consumers can use either NodeNext or bundler module resolution.
