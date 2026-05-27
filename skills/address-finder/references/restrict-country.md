# Restrict search to chosen countries

Narrow the countries you wish to support

Setting this to an empty array (default) will enable all countries

Setting this to a single country disables the country picker control inside the toolbar. To hide the toolbar bar itself, set [`hideToolbar: true`](./hide-toolbar.mdx) — `restrictCountries` and `hideToolbar` are independent options.

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
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
