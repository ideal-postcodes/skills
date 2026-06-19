# Add to your React Application

[`@ideal-postcodes/react`](https://www.npmjs.com/package/@ideal-postcodes/react) is the official integration for Address Finder. It renders an `<input>`, attaches the widget, and hands selected addresses back through callbacks — no `useEffect` or `useRef` wiring required.

## Install

```bash
npm install @ideal-postcodes/react
```

`react` and `react-dom` (`^18` or `^19`) are peer dependencies. The package depends on `@ideal-postcodes/address-finder` internally; you don't need to install it separately.

## Quick Start

The default mode renders its own input. Pass an `apiKey` and capture the selected address via `onAddressRetrieved`.

```tsx
"use client";

import { useState } from "react";
import { AddressFinder } from "@ideal-postcodes/react";
import "@ideal-postcodes/react/css/address-finder.min.css";

type Address = {
  line_1: string;
  line_2: string;
  line_3: string;
  post_town: string;
  postcode: string;
};

const empty: Address = {
  line_1: "",
  line_2: "",
  line_3: "",
  post_town: "",
  postcode: "",
};

export default function CheckoutForm() {
  const [address, setAddress] = useState<Address>(empty);

  return (
    <form>
      <label htmlFor="search">Search address</label>
      <AddressFinder
        id="search"
        apiKey="ak_test"
        defaultCountry="GBR"
        detectCountry={false}
        placeholder="Start typing your address"
        onAddressRetrieved={(a) =>
          setAddress({
            line_1: a.line_1,
            line_2: a.line_2,
            line_3: a.line_3,
            post_town: a.post_town,
            postcode: a.postcode,
          })
        }
      />

      <label>Line 1<input readOnly value={address.line_1} /></label>
      <label>Line 2<input readOnly value={address.line_2} /></label>
      <label>Line 3<input readOnly value={address.line_3} /></label>
      <label>Town<input readOnly value={address.post_town} /></label>
      <label>Postcode<input readOnly value={address.postcode} /></label>
    </form>
  );
}
```

> Replace `ak_test` with your own API key from your [account dashboard](https://account.ideal-postcodes.co.uk/tokens).

## Wrap an Existing Input

Pass a single `<input>` (or any element that renders one and forwards a ref) as `children`. The component attaches the widget to your input rather than rendering its own — useful for design systems like shadcn/ui, MUI, or Mantine, or for any input you've already styled.

```tsx
<AddressFinder
  apiKey="ak_test"
  defaultCountry="GBR"
  onAddressRetrieved={(a) => console.log(a)}
>
  <input
    className="rounded-md border px-3 py-2 w-full"
    placeholder="Search address"
  />
</AddressFinder>
```

In wrap mode, HTML props on `<AddressFinder>` (like `placeholder`, `className`) are ignored — the child owns its own props.

## Common Options

All behavioural options from the underlying controller are supported as props.

```tsx
<AddressFinder
  apiKey="ak_test"
  defaultCountry="GBR"
  restrictCountries={["GBR", "IRL"]}
  hideToolbar={true}
  injectStyle={true}
  queryOptions={{ limit: "10" }}
  resolveOptions={{ tags: ["react-demo"] }}
  removeOrganisation={true}
  titleizePostTown={true}
  onAddressRetrieved={(a) => console.log(a)}
/>
```

- [`defaultCountry`](https://docs.ideal-postcodes.co.uk/docs/address-finder/default-country) - ISO country code applied on mount, e.g. `"GBR"`.
- [`restrictCountries`](https://docs.ideal-postcodes.co.uk/docs/address-finder/restrict-country) - array of ISO codes to restrict the search to.
- [`hideToolbar`](https://docs.ideal-postcodes.co.uk/docs/address-finder/hide-toolbar) — hide the country selector toolbar.
- `injectStyle` - default `true`. Auto-injects the widget stylesheet. See [CSS](#css) below.
- `queryOptions` - forwarded to the [`/autocomplete/addresses`](https://docs.ideal-postcodes.co.uk/docs/api/find-address) endpoint.
- `resolveOptions` - forwarded to the [resolve endpoints](https://docs.ideal-postcodes.co.uk/docs/api/udprn) (e.g. `tags` for usage attribution).
- `detectCountry` - default `true`. Set `false` to skip IP-based country detection.

The legacy DOM-coupled options (`inputField`, `outputFields`, `hide`, `unhide`, `scope`, `inputStyle`, `listStyle`, `mainStyle`, `containerStyle`, `liStyle`) are intentionally absent. React owns the DOM - render your own form fields, update them from `onAddressRetrieved`, and style with CSS classes or your component library.

## Callbacks

Every callback supported by the underlying controller is exposed as a prop. Callbacks always read the latest closure, so you can pass inline functions without `useCallback`.

The three most common:

```tsx
<AddressFinder
  apiKey="ak_test"
  onAddressRetrieved={(address) => {
    // Full address resolved from the API after the user picks a suggestion
  }}
  onAddressSelected={(suggestion) => {
    // User clicked or keyboard-selected a suggestion (before resolve)
  }}
  onSearchError={(error) => {
    // Network or API error during search
  }}
/>
```

See [Callbacks](https://docs.ideal-postcodes.co.uk/docs/address-finder/callbacks) for the full list and semantics (`onAddressPopulated`, `onSuggestionsRetrieved`, `onCountrySelected`, `onOpen`, `onClose`, `onFocus`, `onBlur`, `onFailedCheck`, and more).

## CSS

The widget needs its stylesheet for the suggestion dropdown to render correctly.

By default the component injects the stylesheet into `<head>` on mount:

```tsx
<AddressFinder apiKey="ak_test" />
```

To bundle the CSS yourself (recommended for production builds with stricter CSP), set `injectStyle={false}` and import the stylesheet:

```tsx
import "@ideal-postcodes/react/css/address-finder.min.css";

<AddressFinder apiKey="ak_test" injectStyle={false} />;
```

The unminified `@ideal-postcodes/react/css/address-finder.css` is also exported if you'd like to override variables or compose with your own build pipeline.

## Next.js

In the Next.js App Router, mark any file that uses `<AddressFinder>` as a client component:

```tsx
"use client";

import { AddressFinder } from "@ideal-postcodes/react";
```

Read the API key from an environment variable to avoid hardcoding:

```tsx
<AddressFinder
  apiKey={process.env.NEXT_PUBLIC_IDEAL_POSTCODES_KEY!}
  onAddressRetrieved={(a) => console.log(a)}
/>
```

Pages Router and any React Server Component setup that supports client components work the same way - the widget runs purely in the browser and the component does no work during SSR.
