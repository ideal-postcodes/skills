---
name: ideal-postcodes-react
description: |
  Use this skill whenever the user wants address autocomplete in a React or
  Next.js application, address suggestions as the user types in a React form,
  React address validation, UK address lookup in JSX, or any React component
  that should auto-populate fields from a selected address — even if they
  don't name the package. Covers `@ideal-postcodes/react`: install, the
  `<AddressFinder>` component (default and wrap-around-your-own-input modes),
  callbacks, CSS, country filter/bias, and the React-specific gotchas
  (`"use client"` for Next.js App Router, CSS import path, callbacks always
  see latest closure). For the underlying vanilla JS library, see the
  sibling `address-finder` skill — the option surface is identical except
  for DOM-coupled options.
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
---

# Address Finder for React

React component for the Ideal Postcodes address autocomplete widget. Thin wrapper around the vanilla [`@ideal-postcodes/address-finder`](../ideal-postcodes-address-finder) library — same callbacks, same behavioural options, React-idiomatic surface.

## Quick Start (default mode — component renders its own input)

```bash
npm install @ideal-postcodes/react @ideal-postcodes/address-finder
```

```tsx
"use client";

import { useState } from "react";
import { AddressFinder } from "@ideal-postcodes/react";
import "@ideal-postcodes/react/css/address-finder.min.css";

export default function CheckoutForm() {
  const [address, setAddress] = useState<Record<string, string>>({});

  return (
    <form>
      <AddressFinder
        apiKey="ak_..."
        placeholder="Start typing your address"
        onAddressRetrieved={(a) => setAddress(a)}
      />
      <input value={address.line_1 ?? ""} readOnly />
      <input value={address.post_town ?? ""} readOnly />
      <input value={address.postcode ?? ""} readOnly />
    </form>
  );
}
```

The selected address arrives in `onAddressRetrieved` — caller owns the form fields and updates them via React state. This is intentionally different from the legacy `outputFields` pattern (see "What's different from the vanilla library" below).

## Wrap Mode (caller supplies the input)

Pass an `<input>` (or any component that renders one and accepts a ref) as children — `<AddressFinder>` wraps it instead of rendering its own. Use this when you already have a themed/styled input from your design system.

```tsx
<AddressFinder apiKey="ak_..." onAddressRetrieved={(a) => setAddress(a)}>
  <input
    className="bg-white border rounded px-3 py-2 w-full"
    placeholder="Search address"
  />
</AddressFinder>
```

Works with shadcn `<Input>`, MUI `<TextField>` (via `inputProps`/`InputProps`), Mantine `<TextInput>`, etc. — anything that forwards refs to its underlying `<input>`.

## API

### `<AddressFinder>` props

- **`apiKey`** *(required)* — API key from your Ideal Postcodes account.
- **`children`** *(optional)* — single React element. When present, wraps that element instead of rendering a default input.
- **`inputRef`** — ref forwarded to the rendered `<input>`. Ignored when `children` is provided (put your ref on the child instead).
- **All callbacks** from the underlying `ControllerOptions`: `onAddressRetrieved`, `onAddressSelected`, `onSearchError`, `onSuggestionError`, `onSuggestionsRetrieved`, `onLoaded`, `onFailedCheck`, `onCountrySelected`, `onContextChange`, `onOpen`, `onClose`, `onFocus`, `onBlur`, `onInput`, `onKeyDown`, `onMouseDown`, `onSelect`, `onMounted`, `onRemove`, `onUnhide`, `onAddressPopulated`.
- **Behavioural options**: `defaultCountry`, `restrictCountries`, `queryOptions`, `resolveOptions`, `removeOrganisation`, `titleizePostTown`, `checkKey`, `format`, `hideToolbar`, `detectCountry`, `injectStyle`, `populateCounty`, `populateOrganisation`, `alignToInput`, `offset`, `fixed`, `aria`, and all `msg*` / `*Class` strings. These map 1:1 to the legacy `ControllerOptions` — see the [`address-finder`](../ideal-postcodes-address-finder) skill for full descriptions.
- **HTML input props** (default mode only): `id`, `name`, `className`, `placeholder`, `disabled`, `required`, `aria-label`, `aria-describedby`, `aria-labelledby`.

## What's different from the vanilla library

The React component drops legacy DOM-coupling options. React owns the DOM; the caller renders form fields and reads results from callbacks.

| Vanilla option         | React equivalent                                               |
| ---------------------- | -------------------------------------------------------------- |
| `inputField`           | the rendered `<input>` (or `children` in wrap mode)            |
| `outputFields`         | subscribe to `onAddressRetrieved`, update React state          |
| `hide` / `unhide`      | render output fields conditionally in JSX                      |
| `scope`                | the React subtree itself                                       |
| `inputStyle` / `listStyle` / `mainStyle` / `containerStyle` / `liStyle` | use `className` or override the bundled CSS variables |
| `injectStyle: false` + manual CSS link | `injectStyle={false}` + `import "@ideal-postcodes/react/css/address-finder.min.css"` |

Everything else carries across unchanged.

## Critical Gotchas

- **Next.js App Router needs `"use client"`.** The component uses `useEffect` and DOM refs — it must run client-side. Either add `"use client"` to your page/component file, or import `<AddressFinder>` from a wrapper file that has the directive. Without it you'll get "useEffect is not a function" or "Cannot read properties of null" at build time.

- **CSS doesn't load automatically when bundled.** `injectStyle` defaults to `true`, which injects the stylesheet at runtime — works in plain React apps. For SSR/Next.js where you want CSS in the initial HTML, set `injectStyle={false}` and `import "@ideal-postcodes/react/css/address-finder.min.css"` at app root. Pick one approach; don't do both or you'll get duplicate styles.

- **Callbacks always see the latest closure — don't reach for `useCallback`.** The component stashes the latest props in a ref every render, so `onAddressRetrieved={() => setX(latestState)}` works even when `latestState` updates. Wrapping in `useCallback` is unnecessary and can actually hurt (stale closures if you forget a dep). Just write inline callbacks.

- **`apiKey` is required as a prop.** There's no Provider in v0 — pass `apiKey` directly to every `<AddressFinder>`. Hooks and a Provider may land later; for now, prop or `process.env.NEXT_PUBLIC_IDPC_KEY`-style env var.

- **`apiKey` is camelCase**, not `api_key` or `API_KEY`. Same convention as the vanilla library.

- **Wrap mode requires exactly one child element.** Multiple children, text, or `null` children will throw. The child must render an `<input>` and accept a ref — most styled-input components do this via `React.forwardRef`.

- **The widget reparents the input on mount.** When the widget attaches, it wraps your `<input>` in a `.idpc_autocomplete` container. React still owns the input element (same node reference), but if you're querying the parent in tests or styling based on direct-child selectors, account for the wrapper.

- **API key allowed URLs.** Frontend keys are restricted by domain. If requests fail with 403, check your key's allowed URL list at ideal-postcodes.co.uk/account — match scheme + host (e.g. `https://example.com`, no trailing slash, no path). For local development add `http://localhost:3000` (or your port).

- **StrictMode double-mount is handled.** The component cleans up on unmount and re-attaches on remount — no leaks, no double-attached widgets. You don't need to disable StrictMode.

## Reading the result

`onAddressRetrieved` receives an `AnyAddress` object — a union of GBR (UK) and USA address shapes. For UK addresses, the useful fields are:

```ts
{
  line_1: string;        // "10 Downing Street"
  line_2: string;        // ""
  line_3: string;        // ""
  post_town: string;     // "London"
  postcode: string;      // "SW1A 2AA"
  country: string;       // "England"
  county: string;        // ""
  organisation_name: string;
  // ...plus dozens more — see API docs
}
```

For US addresses (when `format="usa"`), the shape differs — see the `additional-data` reference in the sibling [`address-finder`](../ideal-postcodes-address-finder) skill or [api-typings.ideal-postcodes.co.uk](https://api-typings.ideal-postcodes.co.uk).

## Country handling

Same options as the vanilla library. Most useful:

```tsx
<AddressFinder
  apiKey="ak_..."
  defaultCountry="GBR"        // start here
  restrictCountries={["GBR", "USA", "IRL"]}  // limit the picker
  detectCountry={false}       // disable IP geolocation auto-detect
  hideToolbar                 // remove the country-switcher UI
  onAddressRetrieved={...}
/>
```

See the [`address-finder`](../ideal-postcodes-address-finder) skill for the full set of country options (`filter-by-country`, `bias-by-postcode`, etc.) — they all work as React props.

## Server Components and Streaming

The component is **client-only** — it uses DOM APIs (`useEffect`, refs, mutation) and can't render on the server. Strategies:

- **Next.js App Router**: mark the file `"use client"`. The component will be sent to the client and hydrate there.
- **Next.js Pages Router**: works out of the box (everything is client-rendered after hydration).
- **Remix**: import inside a client-only component or use `<ClientOnly>` from `remix-utils`.
- **Astro**: use `client:load` directive on the React island.

## When `<AddressFinder>` isn't enough

If you need a fully custom UI (your own dropdown, your own suggestion rendering), the hooks layer (`useAutocomplete`, `useResolveAddress`) is on the roadmap but not shipped yet. For now: either use the wrap-mode child slot to insert your themed input, or drop down to `@ideal-postcodes/address-finder` directly and follow the vanilla skill's React pattern.

## Linking out

- Component source: `packages/react-ideal/` in the [`ideal-postcodes/atlas`](https://github.com/ideal-postcodes/atlas) repo
- Vanilla library skill: [`../ideal-postcodes-address-finder`](../ideal-postcodes-address-finder) — read this for the full options reference, filters, biases, and styling
- Live API docs: [docs.ideal-postcodes.co.uk](https://docs.ideal-postcodes.co.uk)
- Address types: [api-typings.ideal-postcodes.co.uk](https://api-typings.ideal-postcodes.co.uk)
