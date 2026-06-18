---
name: ideal-postcodes-address-finder
description: |
  Use this skill whenever the user wants address autocomplete, address
  suggestions as the user types, address validation in a form, UK address
  lookup, or any form field that should be auto-populated from a selected
  address — even if they don't name the package. Specifically covers
  `@ideal-postcodes/address-finder`. Includes npm and CDN install,
  initialisation via `AddressFinder.setup` / `AddressFinder.watch` (React),
  callbacks, CSS, country filter/bias, and common gotchas (API key
  allowed-URLs, bundled vs source build, CSS import).
license: SEE LICENSE IN LICENSE
metadata:
  author: ideal-postcodes
  version: "0.1.0"
  homepage: https://ideal-postcodes.co.uk
  source: https://github.com/ideal-postcodes/skills
inputs:
  - name: IDEAL_POSTCODES_API_KEY
    description: API key for the Ideal Postcodes API. Get one at ideal-postcodes.co.uk/account
    required: true
references:
  - additional-configuration.md
  - additional-data.md
  - bias-by-geolocation.md
  - bias-by-ip.md
  - bias-by-postcode.md
  - callbacks.md
  - configure.md
  - convert-iso-code.md
  - css-classes.md
  - default-country.md
  - default-styling.md
  - detach-reattach-af.md
  - filter-by-country.md
  - filter-by-geospatial-box.md
  - filter-by-locality.md
  - filter-by-postcode-outward.md
  - hide.md
  - hide-toolbar.md
  - home.md
  - how-it-works.md
  - key-usability.md
  - messages.md
  - multiple.md
  - npm.md
  - nudge-address-finder.md
  - omit-organisation.md
  - prevent-autofill.md
  - react.md
  - restrict-country.md
  - script.md
  - separate-input.md
  - single-field.md
  - style-js.md
  - tag-resolve.md
---

# Address Finder

Address autocomplete widget for the Ideal Postcodes API. Suggests addresses as the user types and populates form fields when one is selected.

## Quick Start (npm + bundler)

```bash
npm install @ideal-postcodes/address-finder
```

```js
import { AddressFinder } from "@ideal-postcodes/address-finder";

const controller = AddressFinder.setup({
  apiKey: "ak_test",
  outputFields: {
    line_1: "#line_1",
    line_2: "#line_2",
    line_3: "#line_3",
    post_town: "#post_town",
    postcode: "#postcode",
  },
});
```

If your bundler chokes on the source package, use `@ideal-postcodes/address-finder-bundled` instead — same API, pre-transpiled and polyfilled.

## Quick Start (CDN / drop-in script)

```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled"></script>
<script>
  IdealPostcodes.AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
</script>
```

## React

Use `AddressFinder.watch` (not `.setup`) inside `useEffect` — it will pick up the input field once it mounts. See [`react.md`](./references/react.md).

## Critical Gotchas

- **Use `AddressFinder.setup({...})` — not `new AddressFinder(...)`.** It's a factory method, not a constructor.
- **`apiKey` is camelCase**, not `api_key` or `API_KEY`.
- **`outputFields` is required** — it's the map from address attributes (`line_1`, `post_town`, `postcode`, …) to your form's input selectors. Without it, the widget has nothing to populate.
- **Pin your CDN version in production.** Pulling from `@latest` will silently break when a major bumps. Use `@ideal-postcodes/address-finder-bundled@4` (or whatever current major).
- **Source vs bundled package:** use `@ideal-postcodes/address-finder` for bundlers (smaller, tree-shakable). Use `@ideal-postcodes/address-finder-bundled` for `<script>` tags or if your bundler can't handle the source package.
- **API key allowed URLs.** Frontend keys are restricted by domain. If requests fail with 403, check your key's allowed URL list at ideal-postcodes.co.uk/account — match scheme + host (e.g. `https://example.com`, no trailing slash, no path).
- **Country toolbar vs country restriction are independent.** `restrictCountries: ["GBR"]` removes the country *picker control* inside the toolbar — it does **not** hide the toolbar bar itself. For a clean single-country setup with no toolbar on focus, also set `hideToolbar: true`. See [`hide-toolbar.md`](./references/hide-toolbar.md).
- **Bundler CSS import.** The source package does not auto-inject CSS. Either import the stylesheet (`@ideal-postcodes/address-finder/css/address-finder.css`) or set `injectStyle: true` in the setup options.

For the long tail of error patterns and fixes, see [`troubleshooting.md`](./troubleshooting.md).

## When to read which reference

The references below are organised by intent. Read only the ones relevant to the user's task — not the whole list.

### Setup and initialisation
- [`npm.md`](./references/npm.md) — npm/bundler install, source vs bundled trade-offs
- [`script.md`](./references/script.md) — CDN `<script>` tag install
- [`configure.md`](./references/configure.md) — minimum required config (`apiKey`, `outputFields`)
- [`additional-configuration.md`](./references/additional-configuration.md) — full options reference
- [`callbacks.md`](./references/callbacks.md) — `onAddressRetrieved`, `onLoaded`, `onCheckFailed`, etc.

### React / single-page apps
- [`react.md`](./references/react.md) — `AddressFinder.watch` + `useEffect` pattern
- [`detach-reattach-af.md`](./references/detach-reattach-af.md) — re-mount on route change
- [`nudge-address-finder.md`](./references/nudge-address-finder.md) — manual re-init / re-attach

### Country handling
- [`restrict-country.md`](./references/restrict-country.md) — limit which countries the user can pick (removes the picker, not the toolbar bar)
- [`hide-toolbar.md`](./references/hide-toolbar.md) — hide the toolbar bar entirely (`hideToolbar: true`); independent of `restrictCountries`
- [`default-country.md`](./references/default-country.md) — pre-select / bias toward a country
- [`filter-by-country.md`](./references/filter-by-country.md) — filter results by country
- [`convert-iso-code.md`](./references/convert-iso-code.md) — ISO 3166 helper

### Filter / bias suggestions
- [`filter-by-locality.md`](./references/filter-by-locality.md) — by post town
- [`filter-by-postcode-outward.md`](./references/filter-by-postcode-outward.md) — by outward postcode
- [`filter-by-geospatial-box.md`](./references/filter-by-geospatial-box.md) — within a bounding box
- [`bias-by-postcode.md`](./references/bias-by-postcode.md) — toward a postcode
- [`bias-by-geolocation.md`](./references/bias-by-geolocation.md) — toward a lat/lng
- [`bias-by-ip.md`](./references/bias-by-ip.md) — toward the user's IP location

### Styling and UI
- [`default-styling.md`](./references/default-styling.md) — built-in look
- [`css-classes.md`](./references/css-classes.md) — class hooks for custom CSS
- [`style-js.md`](./references/style-js.md) — programmatic styling
- [`messages.md`](./references/messages.md) — customise UI strings

### Form layout patterns
- [`single-field.md`](./references/single-field.md) — one input that captures the whole address
- [`separate-input.md`](./references/separate-input.md) — dedicated finder input separate from output fields
- [`multiple.md`](./references/multiple.md) — more than one finder on the page
- [`hide.md`](./references/hide.md) — hide output fields until an address is picked
- [`prevent-autofill.md`](./references/prevent-autofill.md) — disable browser autofill on finder input
- [`key-usability.md`](./references/key-usability.md) — keyboard navigation tweaks

### Address data
- [`additional-data.md`](./references/additional-data.md) — populate fields beyond the standard 5 lines
- [`omit-organisation.md`](./references/omit-organisation.md) — strip organisation name from line 1
- [`tag-resolve.md`](./references/tag-resolve.md) — tag-based address resolution

### Concepts
- [`how-it-works.md`](./references/how-it-works.md) — internal model: input field, dropdown, key check

### Troubleshooting
- [`troubleshooting.md`](./troubleshooting.md) — common errors with root cause + fix (authored sibling, not in references/)

## Full documentation

The full Ideal Postcodes documentation — every guide, API reference, and integration — is available as a single file at [llms.txt](https://docs.ideal-postcodes.co.uk/llms.txt). Point your agent there for anything this skill doesn't cover.
