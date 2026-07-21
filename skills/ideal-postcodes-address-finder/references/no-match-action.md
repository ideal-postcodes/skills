# Offer an Out When No Matches Are Found

## No-Match Action

Sometimes a user genuinely cannot find their address - a new build, an unusual spelling, or an address not yet in the dataset. Without an escape hatch they are stuck at `"No matches found"`.

Address Finder can render an actionable item beneath the no-match message. Use it to hand the user off to a fallback, most commonly a manual address entry form.

## Enable

Set both options - the item only renders when the label and the callback are configured:

- [`msgNoMatchAction`](https://docs.ideal-postcodes.co.uk/docs/address-finder/messages) - the label, e.g. `"Enter address manually"`. Defaults to `""` (disabled).
- [`onNoMatchAction`](https://docs.ideal-postcodes.co.uk/docs/address-finder/callbacks) - invoked when the user selects the item. Address Finder closes on selection.

```javascript
AddressFinder.setup({
  apiKey: "ak_test",
  inputField: "#search",
  msgNoMatchAction: "Enter address manually",
  onNoMatchAction: function () {
    // e.g. reveal a manual address entry form
  },
});
```

The item behaves like a suggestion: it can be clicked, or highlighted with the arrow keys and selected with Enter, and it is announced to screen readers as an option.

## Styling

The item carries the `idpc_action` class (configurable via `noMatchActionClass`). The default stylesheet centers and underlines it - override the class to restyle. See [CSS Classes](https://docs.ideal-postcodes.co.uk/docs/address-finder/css-classes).

## Live Demo

Type nonsense (e.g. `zzzzzz`) to trigger the no-match message, then select *Enter address manually*.

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="search">Search Your Address</label>
    <input type="text" id="search" placeholder="Start typing address here" />
    <fieldset id="manual" hidden>
        <legend>Manual entry</legend>
        <label for="line_1">Address First Line</label>
        <input type="text" id="line_1" />
        <label for="line_2">Address Second Line</label>
        <input type="text" id="line_2" />
        <label for="line_3">Address Third Line</label>
        <input type="text" id="line_3" />
        <label for="post_town">Town or City</label>
        <input type="text" id="post_town" />
        <label for="postcode">Postcode</label>
        <input type="text" id="postcode" />
    </fieldset>
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  const manual = document.getElementById("manual");

  AddressFinder.setup({
    apiKey: "ak_test",
    inputField: "#search",
    msgNoMatchAction: "Enter address manually",
    onNoMatchAction: function () {
      manual.hidden = false;
      document.getElementById("line_1").focus();
    },
    onAddressPopulated: function () {
      manual.hidden = false;
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
