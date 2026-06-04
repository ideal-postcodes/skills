---
name: ideal-postcodes-postcode-lookup
description: |
  Use this skill whenever the user wants UK postcode lookup, postcode search,
  "find addresses by postcode", a postcode-to-address dropdown, or any UK
  address form where the user enters a postcode and picks an address — even
  if they don't name the package. Specifically covers
  `@ideal-postcodes/postcode-lookup`. Includes npm and CDN install,
  initialisation via `PostcodeLookup.setup`, lifecycle callbacks, CSS
  customisation, full-text address search, custom UI elements, and common
  gotchas (the required `context` parameter, API key allowed-URLs, bundled
  vs source build).
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
  - add-custom-elements.md
  - additional-configuration.md
  - additional-data.md
  - behaviours.md
  - callbacks.md
  - configure.md
  - css-classes.md
  - custom-button.md
  - custom-callback.md
  - custom-elements.md
  - custom-input.md
  - full-search.md
  - hide-fields.md
  - home.md
  - how-it-works.md
  - messages.md
  - multi-page-form.md
  - multiple.md
  - npm.md
  - react.md
  - remove-organisation.md
  - script.md
  - single-field.md
  - style-tweaks.md
---

# Postcode Lookup

Postcode search widget for the Ideal Postcodes API. User enters a UK postcode, picks an address from the dropdown, and your form fields auto-populate.

## Quick Start (npm + bundler)

```bash
npm install @ideal-postcodes/postcode-lookup
```

```js
import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

const controller = PostcodeLookup.setup({
  apiKey: "ak_test",
  context: "#postcode-lookup-container",
  outputFields: {
    line_1: "#line_1",
    line_2: "#line_2",
    line_3: "#line_3",
    post_town: "#post_town",
    postcode: "#postcode",
  },
});
```

```html
<div id="postcode-lookup-container"></div>
```

## Quick Start (CDN / drop-in script)

```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@2"></script>
<div id="postcode-lookup-container"></div>
<script>
  IdealPostcodes.PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#postcode-lookup-container",
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

## Critical Gotchas

- **`context` is required.** It tells the widget where to render its UI (search box, dropdown). Pass a CSS selector or DOM node. Without it, nothing appears.
- **Use `PostcodeLookup.setup({...})` — not `new PostcodeLookup(...)`.** It's a factory method, not a constructor.
- **`apiKey` is camelCase**, not `api_key` or `API_KEY`.
- **Pin your CDN version in production.** Pulling from `@latest` will silently break on a major. Use `@ideal-postcodes/postcode-lookup-bundled@2` (or current major).
- **Source vs bundled package:** use `@ideal-postcodes/postcode-lookup` for bundlers. Use `@ideal-postcodes/postcode-lookup-bundled` for `<script>` tags or compatibility.
- **API key allowed URLs.** Frontend keys are restricted by domain. If requests fail with 403, add your domain at ideal-postcodes.co.uk/account — match scheme + host (e.g. `https://example.com`).
- **Postcode normalization is automatic.** The widget handles whitespace and case. Do not pre-format input.

For the long tail of error patterns and fixes, see [`troubleshooting.md`](./troubleshooting.md).

## When to read which reference

The references below are organised by intent. Read only the ones relevant to the user's task — not the whole list.

### Setup and initialisation
- [`npm.md`](./references/npm.md) — npm/bundler install, source vs bundled trade-offs
- [`script.md`](./references/script.md) — CDN `<script>` tag install
- [`configure.md`](./references/configure.md) — minimum required config (`apiKey`, `context`, `outputFields`)
- [`additional-configuration.md`](./references/additional-configuration.md) — full options reference
- [`callbacks.md`](./references/callbacks.md) — `onAddressSelected`, `onLoaded`, `onSearchCompleted`, etc.

### React / single-page apps
- [`react.md`](./references/react.md) — useEffect-based init pattern

### Search behaviour
- [`full-search.md`](./references/full-search.md) — enable address-text search alongside postcode (`strictlyPostcodes: false`)
- [`behaviours.md`](./references/behaviours.md) — fine-grained behaviour flags
- [`single-field.md`](./references/single-field.md) — single combined input

### Form patterns
- [`multiple.md`](./references/multiple.md) — more than one postcode lookup on one page
- [`multi-page-form.md`](./references/multi-page-form.md) — populate fields across multiple form pages
- [`hide-fields.md`](./references/hide-fields.md) — hide output fields until an address is picked
- [`remove-organisation.md`](./references/remove-organisation.md) — strip organisation name from line 1

### Custom UI elements
- [`custom-input.md`](./references/custom-input.md) — replace the default postcode input
- [`custom-button.md`](./references/custom-button.md) — replace the default search button
- [`custom-elements.md`](./references/custom-elements.md) — replace any rendered element
- [`add-custom-elements.md`](./references/add-custom-elements.md) — inject extra UI (e.g. manual-entry link)
- [`custom-callback.md`](./references/custom-callback.md) — drive selection from your own UI

### Styling and UI
- [`css-classes.md`](./references/css-classes.md) — class hooks for custom CSS
- [`style-tweaks.md`](./references/style-tweaks.md) — common CSS overrides
- [`messages.md`](./references/messages.md) — customise UI strings

### Address data
- [`additional-data.md`](./references/additional-data.md) — populate fields beyond the standard 5 lines

### Concepts
- [`how-it-works.md`](./references/how-it-works.md) — internal model: search → dropdown → populate

### Troubleshooting
- [`troubleshooting.md`](./troubleshooting.md) — common errors with root cause + fix (authored sibling, not in references/)
