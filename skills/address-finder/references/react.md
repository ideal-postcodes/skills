# Add to your React Application

This example deploys Address Finder with React.

## Install

```bash
npm install --save @ideal-postcodes/address-finder
```

## Import Address Finder

Add the following to your component.

```javascript
import { AddressFinder } from "@ideal-postcodes/address-finder";
import { useEffect, useState, useRef } from "react";
```

## Initializing Component State

Within your component, use a state hook to capture the address elements you need.

```javascript
const [state, setState] = useState({
  line_1: "",
  line_2: "",
  line_3: "",
  post_town: "",
  postcode: "",
  country: "",
});
```

## Initialise Address Finder within useEffect

```javascript
// The useRef hook is used to persist the flag shouldRender across re-renders, ensuring the address finder is initialized only once.
const shouldRender = useRef(true);

useEffect(() => {
  if (!shouldRender.current) return;
  shouldRender.current = false;

  AddressFinder.watch({
    inputField: "#searchField",
    apiKey: "ak_test",
    onAddressRetrieved: (address) => {
      setState({
        line_1: address.line_1,
        line_2: address.line_2,
        line_3: address.line_3,
        post_town: address.post_town,
        postcode: address.postcode,
        country: address.country,
      });
    },
  });
}, []);
```

> Make sure to replace the `apiKey` `ak_test` with your own API key. This can be located on your [account](https://account.ideal-postcodes.co.uk/tokens).

## Your Address Inputs

Your search field should have an `id` which matches `inputField` (in this instance it's `"searchField"`).

You can create a typical React form by binding the component's state to the relevant input field.

```javascript
return (
  <form className="App">
    <h1>Address Finder React Demo</h1>
    <div>
      <input
        placeholder="Start typing to find your address"
        id="searchField"
        type="text"
      />
    </div>
    <div>
      <label htmlFor="line_1">Address Line One</label>
      <input
        type="text"
        value={state.line_1}
        onChange={(e) => setState({ ...state, line_1: e.target.value })}
      />
    </div>
    {/* Similar divs for other address parts */}
  </form>
);
```

> If you wish to add an additional field, include the parameter name [from our documentation](https://docs.ideal-postcodes.co.uk/docs/data/paf).
