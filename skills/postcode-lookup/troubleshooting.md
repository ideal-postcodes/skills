# Troubleshooting Postcode Lookup

The recurring failure modes when integrating
`@ideal-postcodes/postcode-lookup`, each with the cause and the fix. If the
user is hitting one of these, jump straight to the matching section — don't
re-derive the diagnosis.

## Initialisation problems

### Nothing appears on the page

**Cause:** missing or invalid `context`. `PostcodeLookup` needs a container
element to render into. Without it, the widget initialises silently but
nothing is visible.

**Fix:** ensure the `context` selector / element resolves at the time
`setup` runs. Add a `<div id="...">` to the markup:

```html
<div id="postcode-lookup-container"></div>
```

```js
PostcodeLookup.setup({
  apiKey: "ak_test",
  context: "#postcode-lookup-container",
  outputFields: { /* ... */ },
});
```

If the container is rendered conditionally or after route change, defer
`setup` until it exists, or use the `onLoaded` callback to confirm the
controller bound correctly.

### `PostcodeLookup is not a constructor`

**Cause:** code is calling `new PostcodeLookup(...)`. It's a factory, not a
class.

**Fix:** `PostcodeLookup.setup({...})`.

### Widget is unstyled / looks broken

**Cause:** missing CSS. The source package
(`@ideal-postcodes/postcode-lookup`) does not auto-inject styles.

**Fix:** import the stylesheet
(`import "@ideal-postcodes/postcode-lookup/css/postcode-lookup.css"`) or
swap to the bundled package (`@ideal-postcodes/postcode-lookup-bundled`)
which injects automatically.

### Bundler errors importing the source package

**Cause:** bundler can't process modern JS in `node_modules`.

**Fix:** swap to `@ideal-postcodes/postcode-lookup-bundled` — same API,
pre-transpiled and polyfilled.

## Network errors

### 403 Forbidden

**Cause:** the API key has an allowed-URL list and the page's origin isn't
on it. Frontend keys are domain-restricted by default.

**Fix:** at `https://ideal-postcodes.co.uk/account` → Keys → click the key
→ Allowed URLs, add the exact origin (scheme + host, no trailing slash).
Add `http://localhost:<port>` for dev.

### 401 Unauthorized

**Cause:** wrong, deleted, or paused key. Or using a secret key (`sk_*`)
on the frontend.

**Fix:** verify against the dashboard. Frontend = `ak_*`, server =
`sk_*`.

### 429 Too Many Requests

**Cause:** daily lookup balance exhausted, or per-second rate limit hit.

**Fix:** check account dashboard. Top up or contact support to raise the
cap.

### Content Security Policy (CSP) blocks the widget

**Cause:** page CSP doesn't allow the widget's script source or API
endpoint.

**Fix:**
- `connect-src`: `https://api.ideal-postcodes.co.uk`
- `script-src` (CDN install): `https://cdn.jsdelivr.net`
- `style-src` (with auto-inject): import the stylesheet via your bundler
  instead, to avoid `'unsafe-inline'`.

## Search behaviour

### "No results found" for a real postcode

**Cause:** typically a typo, but occasionally the postcode is valid but the
key's data licence excludes that area (rare; only happens on restricted
licences).

**Fix:** verify the postcode against `https://api.postcodes.io/postcodes/<pc>`
(free, public). If it's there but Ideal Postcodes returns empty, contact
support — your licence may be restricted.

### User can't search by address text, only postcode

**Cause:** `strictlyPostcodes` defaults to `true` — the widget treats input
as a postcode only.

**Fix:** set `strictlyPostcodes: false` in the setup options. See
[`full-search.md`](./references/full-search.md).

### Postcode is rejected even though it looks right

**Cause:** the widget validates UK postcode format before searching. If the
user's input doesn't match, it short-circuits.

**Fix:** the widget normalises whitespace and case automatically, so don't
pre-format. If a clearly-valid postcode is rejected, log the input — it's
usually trailing whitespace or a hidden character (e.g. paste from PDF).

## Output / population

### `outputFields` selectors look right but fields don't populate

**Cause:** the selector matches a different element than expected, or the
field is hidden / disabled at the moment of population.

**Fix:** open devtools, run the same selector
(`document.querySelector("#line_1")`), confirm it returns the expected
element. Check no other JS clears the field after population. The
`onAddressSelected` callback receives the full address payload — log it to
confirm the data is arriving.

### Want to populate fields beyond the standard 5

**Fix:** see [`additional-data.md`](./references/additional-data.md). Any
field on the API response (`thoroughfare`, `dependant_locality`,
`organisation_name`, `udprn`, …) can be mapped via `outputFields` or read
in `onAddressSelected`.

### Multi-page form: address selected on page 1, fields are on page 2

**Cause:** by default, `outputFields` selectors are evaluated when an
address is selected. If the target inputs aren't in the DOM yet, nothing
populates.

**Fix:** see
[`multi-page-form.md`](./references/multi-page-form.md). Capture the
selected address in `onAddressSelected`, store it (state, sessionStorage),
and apply on page 2 mount.

## React-specific

### Widget appears twice / `onLoaded` fires twice in dev

**Cause:** React 18 StrictMode mounts effects twice in development.

**Fix:** guard with a ref:

```jsx
const inited = useRef(false);
useEffect(() => {
  if (inited.current) return;
  inited.current = true;
  PostcodeLookup.setup({ context: "#pl", apiKey: "ak_test", ... });
}, []);
```

### Stale controller after route change

**Cause:** the `context` element gets unmounted on navigation but the
controller still references the old node.

**Fix:** call `controller.unbind()` on cleanup, then re-`setup` on the new
mount:

```jsx
useEffect(() => {
  const c = PostcodeLookup.setup({ /* ... */ });
  return () => c.unbind();
}, []);
```

## When the gotcha isn't here

1. Check `onLoaded` and `onCheckFailed` — those tell the page if the key
   passed the runtime check.
2. Open the network tab, inspect `/v1/postcodes/<postcode>` — error
   responses include a `code` and `message`.
3. Reach support: `support@ideal-postcodes.co.uk`. Include the API key
   (the public `ak_*` is fine), a reproducible URL, and a request ID from
   the network tab.
