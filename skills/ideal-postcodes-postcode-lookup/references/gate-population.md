# Gate Population by Delivery Area

## Overview

`shouldAddressPopulate` runs when the user picks an address from the dropdown, **before** it is written to the form. Return `false` to cancel population, or `true` to proceed. Use it to block addresses you can't serve - outside a delivery area, an unsupported country, or any custom rule - before the fields are filled in.

The predicate may be synchronous or return a promise, so you can call your own API to make the decision.

## Cancel synchronously

Return `false` from a plain function to block population. Here we only accept London (`E`, `EC`, `N`, `NW`, `SE`, `SW`, `W`, `WC`) postcodes:

```javascript
import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

const LONDON = /^(EC|WC|E|N|NW|SE|SW|W)/i;

PostcodeLookup.setup({
  context: "#lookup_field",
  apiKey: "ak_test",
  outputFields: {
    line_1: "#first_line",
    post_town: "#post_town",
    postcode: "#postcode",
  },
  shouldAddressPopulate: function (address) {
    const ok = LONDON.test(address.postcode);
    if (!ok) this.setMessage("Sorry, we only deliver within London");
    return ok;
  },
});
```

## Check asynchronously

Return a promise to defer to your own API. **Errors are your responsibility**: a rejecting promise **fails open** (population proceeds) rather than blocking the user on a broken check. Catch inside the predicate and return an explicit boolean - `return false` to cancel, `return true` to allow:

```javascript
shouldAddressPopulate: async function (address) {
  try {
    const res = await fetch(`/api/delivery-area?postcode=${address.postcode}`);
    const { deliverable } = await res.json();
    if (!deliverable) this.setMessage("Sorry, we don't deliver to this address");
    return deliverable;
  } catch (err) {
    reportError(err);   // your telemetry - the widget will not surface this
    return true;        // fail open: don't block the customer on an outage
    // return false;    // ...or fail closed to be strict
  }
}
```

## Reacting to the decision

- **To show a message on cancel**, do it inside the predicate before `return false` (as above).
- **`onAddressSelected` still fires on cancel** - it runs whether or not population proceeds, so use it for "the user chose this address" analytics, not as a signal that fields were filled.
- **To confirm population actually happened** - for example to gate a form submit - use the [`onAddressPopulated`](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks) callback. It fires only when population proceeds.

## Live Demo

Search a postcode and pick an address. Non-London postcodes are declined and the fields stay empty.

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
    <label>Address Line One</label>
    <input id="first_line" type="text" />
    <label>Post Town</label>
    <input id="post_town" type="text" />
    <label>Postcode</label>
    <input id="postcode" type="text" />
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  const LONDON = /^(EC|WC|E|N|NW|SE|SW|W)/i;

  PostcodeLookup.setup({
    context: "#lookup_field",
    apiKey: "ak_test",
    outputFields: {
      line_1: "#first_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
    shouldAddressPopulate: function (address) {
      const ok = LONDON.test(address.postcode);
      if (!ok) this.setMessage("Sorry, we only deliver within London");
      return ok;
    },
  });
```
