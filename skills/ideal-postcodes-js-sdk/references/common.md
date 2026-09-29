# Responses, cancellation and bundles

Every operation resolves to `{ data, request, response }`, accepts an `AbortSignal` and ships as a named ESM export that bundlers can drop when unused.

## What does an operation return?

A successful JSON response is in `data`. `keyLogs` returns CSV text instead. `request` and `response` are the fetch `Request` and `Response`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/typescript/errors) for failures.

## How do I cancel a request?

Pass an `AbortSignal` as `signal`. Aborting rejects the call with `AbortError`.

```ts title="cancel.ts"
const controller = new AbortController();
const pending = findAddress({
  client,
  query: { query: "10 Downing Street" },
  signal: controller.signal,
});
controller.abort();
await pending.catch(console.error);
```

## How do I import the types?

Import request and response types with `import type`, for example `FindAddressData` and `FindAddressResponse`. The flat `Address` model includes a `native` union for dataset-specific fields.

## Does the SDK tree-shake?

Yes, with named ESM imports. A bundler removes the operations you do not import. The fetch transport is shared overhead. Autocomplete and the React/Preact adapters live behind separate subpaths. Bare imports do not include those helpers or framework runtimes.
