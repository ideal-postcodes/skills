# Troubleshooting Address Finder

The recurring failure modes when integrating `@ideal-postcodes/address-finder`,
each with the cause and the fix. If the user is hitting one of these, jump
straight to the matching section — don't re-derive the diagnosis.

## Network errors

### 403 Forbidden on every request

**Cause:** the API key has an allowed-URL list and the page's origin isn't on
it. Frontend keys (`ak_*`) are domain-restricted by default — Ideal Postcodes
checks the `Origin`/`Referer` of every request.

**Fix:** at `https://ideal-postcodes.co.uk/account` → Keys → click the key →
Allowed URLs, add the exact origin. Match scheme and host (e.g.
`https://example.com`, no trailing slash, no path). For local dev, add
`http://localhost:3000` (or whatever port). Don't store the test key
`ak_test` in production — it has its own allowed-URL list and is shared.

### 401 Unauthorized

**Cause:** the key is wrong, deleted, or paused.

**Fix:** verify the value in `apiKey` matches the key shown in the dashboard.
Common mistake: copying with surrounding whitespace, or using the *secret*
key (`sk_*`) on the frontend (those are server-only).

### 429 Too Many Requests

**Cause:** either daily lookup balance is exhausted, or per-second rate
limit hit (typically only seen with bot traffic or load tests).

**Fix:** check the account dashboard for balance and rate-limit settings.
For development, top up or use the test key. For production bursts,
contact support to raise the per-key cap.

### CORS error in browser console

**Cause:** mismatch between the origin the browser is sending and what's on
the key's allowed list. Or you're calling from a non-HTTPS origin to the
HTTPS API.

**Fix:** add the exact origin (with scheme) to the key's allowed URLs. For
local file:// development, that won't work — serve over `http://localhost`.

### Content Security Policy (CSP) blocks the widget

**Cause:** the page's `Content-Security-Policy` header doesn't allow the
finder's script source or its API endpoint.

**Fix:** add to `connect-src`: `https://api.ideal-postcodes.co.uk`. If using
the CDN bundle, add to `script-src`: `https://cdn.jsdelivr.net`. If
`injectStyle: true`, add to `style-src`: `'unsafe-inline'`, or import the
stylesheet via your bundler instead.

## Initialisation problems

### `AddressFinder is not a constructor`

**Cause:** code is calling `new AddressFinder(...)`. It's a factory, not a
class.

**Fix:** `AddressFinder.setup({...})` (or `AddressFinder.watch({...})` for
React).

### `outputFields is required` / silent: form not populating after selection

**Cause:** missing or wrong `outputFields`. The widget retrieves the address
but has nowhere to put it.

**Fix:** map every field you want populated. Selectors are evaluated lazily,
so they can refer to fields rendered after setup runs:

```js
outputFields: {
  line_1: "#line_1",
  line_2: "#line_2",
  line_3: "#line_3",
  post_town: "#post_town",
  postcode: "#postcode",
}
```

If selectors look right but fields still don't update, open devtools — the
selector probably matches a different element than expected. The
`onAddressRetrieved` callback receives the full address payload; logging it
confirms the data is arriving.

### Widget invisible but no errors

**Cause:** missing CSS. The source package
(`@ideal-postcodes/address-finder`) does not auto-inject styles.

**Fix:** either import the stylesheet
(`import "@ideal-postcodes/address-finder/css/address-finder.css"`) or set
`injectStyle: true` in the setup options. The bundled package
(`-bundled`) injects automatically.

### Bundler errors importing the source package

**Cause:** bundler can't process modern JS in `node_modules` (typical with
older Webpack / Vite configs that don't transpile dependencies).

**Fix:** swap to `@ideal-postcodes/address-finder-bundled` — same API,
pre-transpiled and polyfilled. Slightly larger output but config-free.

## React-specific

### Widget appears twice / `onLoaded` fires twice in dev

**Cause:** React 18 StrictMode mounts components twice in development. Each
mount calls `setup()` / `watch()` again.

**Fix:** guard with a ref so init runs once even on re-mount:

```jsx
const inited = useRef(false);
useEffect(() => {
  if (inited.current) return;
  inited.current = true;
  AddressFinder.watch({ inputField: "#search", apiKey: "ak_test", ... });
}, []);
```

This is also the right pattern outside StrictMode — you don't want
re-initialisation on every render anyway.

### `setup` runs but no autocomplete appears

**Cause:** the input field referenced by `inputField` (or by the implicit
selector binding) hadn't mounted when `setup` ran.

**Fix:** use `AddressFinder.watch` instead of `.setup`. `watch` polls the
DOM and binds when the field appears. See
[`react.md`](./references/react.md).

### Page navigation: widget stops working after route change

**Cause:** in SPAs, the input field gets unmounted on navigation but the
controller still references the old element.

**Fix:** call `controller.refresh()` after navigation, or use `watch`. See
[`detach-reattach-af.md`](./references/detach-reattach-af.md) and
[`nudge-address-finder.md`](./references/nudge-address-finder.md).

## UX issues

### Browser autofill collides with the finder dropdown

**Cause:** the finder input gets `autocomplete` suggestions from the browser
on top of the widget's dropdown.

**Fix:** see [`prevent-autofill.md`](./references/prevent-autofill.md) —
the finder accepts an option to disable browser autofill on its input.

### Suggestions list is empty for valid input

**Cause:** a country filter, postcode filter, or geospatial box is active
and excluding the user's address.

**Fix:** check the filter options against where the user is actually
typing. `filter` is restrictive; `bias` only re-orders results. Loosen
filter or switch to bias.

### Postcodes appear but house number doesn't filter the list

**Cause:** the source package returns postcode-level matches by default;
house-number disambiguation requires the finder to be configured for it.

**Fix:** the address-finder is autocomplete-style — it matches as the user
types. For postcode-then-house-number flow, use Postcode Lookup instead
(separate skill).

## When the gotcha isn't here

1. Check the `onCheckFailed` callback fired during init — that's the API's
   way of telling the page the key isn't usable.
2. Open the network tab and inspect a `/v1/autocomplete/addresses` request.
   The error body has a machine-readable `code` and human-readable `message`.
3. Reach support: `support@ideal-postcodes.co.uk`. Include the API key (the
   public `ak_*` form is fine), the URL where the problem reproduces, and a
   request ID from the network tab.
