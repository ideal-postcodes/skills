# Responses, cancellation and bundles

Every operation resolves to `{ data, request, response }`, accepts an `AbortSignal` and ships as a named ESM export that bundlers can drop when unused.

## Return values

A successful JSON response is in `data`. `keyLogs` returns CSV text instead. `request` and `response` are the fetch `Request` and `Response`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors) for failures.

## Cancel a request

Pass an `AbortSignal` as `signal`. Aborting rejects the call with `AbortError`.

```ts title="cancel.ts"
const controller = new AbortController();
const pending = findAddress({
  client,
  query: { query: "9 Malyons Road" },
  signal: controller.signal,
});
controller.abort();
await pending.catch(console.error);
```

To cap how long every request may run, set [`timeoutMs`](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/setup#request-timeout) on the client.

## Types

Import request and response types with `import type`, for example `FindAddressData` and `FindAddressResponse`. The flat `Address` model includes a `native` union for dataset-specific fields.

## Tree-shaking

Named ESM imports let a bundler remove the operations you do not import. The fetch transport is shared overhead. Autocomplete and the React/Preact adapters live behind separate subpaths. Bare imports do not include those helpers or framework runtimes.
