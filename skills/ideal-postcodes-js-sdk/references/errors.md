# Error handling

An HTTP error rejects with `ApiError`, a subclass of `Error`. A network failure or an aborted request rejects with the runtime's native error.

## What does an ApiError contain?

`ApiError` carries the HTTP status, the API error code and the parsed body.

| Property | Contents |
| --- | --- |
| `status` | HTTP status, such as `404` |
| `code` | API error code, such as `4040`. `undefined` when the body is not JSON |
| `message` | API error message, or the HTTP status text |
| `body` | Parsed JSON body, or the raw text |
| `response` | The fetch `Response` |

```ts title="errors.ts"
import { ApiError, createIdpcClient, postcodes } from "@ideal-postcodes/sdk";

const client = createIdpcClient({ apiKey: "ak_test" });
try {
  const { data } = await postcodes({ client, path: { postcode: "SW1A 2AX" } });
  console.log(data.result);
} catch (e) {
  if (!(e instanceof ApiError)) throw e;
  if (e.code === 4040) console.log("Postcode not found", e.body);
  else console.error(e.status, e.code, e.message);
}
```

`ApiError` covers HTTP errors only. A network failure or an aborted request rejects with the runtime's native error, such as `TypeError` or `AbortError`. The [error codes guide](https://docs.ideal-postcodes.co.uk/docs/guides/error-codes) lists each code and how to fix it.

## How do I handle errors without try/catch?

Pass `throwOnError: false` to get `{ data, error }` instead of a rejection. For an HTTP error, `error` is the API's error body, typed per operation. `response` holds the HTTP status. For a network failure, `error` is the native error and `response` is `undefined`.

```ts title="result.ts"
const result = await postcodes({ client, path: { postcode: "SW1A 2AX" }, throwOnError: false });
if (result.error) {
  if ("suggestions" in result.error) console.log(result.error.suggestions);
  else console.error(result.response?.status, result.error.code, result.error.message);
} else {
  console.log(result.data.result);
}
```
