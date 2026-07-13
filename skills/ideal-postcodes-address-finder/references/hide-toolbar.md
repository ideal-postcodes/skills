# Hide the country toolbar

Suppress the toolbar that renders below the suggestion list on focus.

`hideToolbar` is **independent** of [`restrictCountries`](https://docs.ideal-postcodes.co.uk/docs/address-finder/restrict-country). `restrictCountries: ["GBR"]` removes the country *picker control* inside the toolbar but the toolbar container still renders. To hide the bar entirely — useful for single-country checkouts that want a clean, "nothing shows until results" focus experience — set `hideToolbar: true`.

Defaults to `false`.

## Live Demo

```html
<form>
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
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    detectCountry: false,
    defaultCountry: "GBR",
    restrictCountries: ["GBR"],
    hideToolbar: true,
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
