# Populate a country select

Your form may capture country with a `<select>` rather than a free text `<input>` - for example a checkout that stores the ISO-3 country code (`"GBR"`, `"IRL"`, `"FRA"`) against the order.

Point an `outputFields` country attribute at the `<select>` and Address Finder selects the matching option for you. It matches the retrieved value against each option's `value` first, then falls back to the option's visible text, so you don't need a callback.

Map the attribute whose values line up with your option `value`s:

- `country_iso` for 3 letter ISO codes (`"GBR"`)
- `country_iso_2` for 2 letter ISO codes (`"GB"`)
- `country` for the full country name (`"United Kingdom"`)

## Live Demo

The country `<select>` below uses 3 letter ISO codes as its option values, so `country_iso` is mapped to it. Search for a UK address and the `United Kingdom` option is selected automatically.

```html
<form style="max-width: 450px; padding: 10px;">
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
  <label for="country">Country</label>
  <select id="country" name="country">
    <option value="">Select a country</option>
    <option value="FRA">France</option>
    <option value="DEU">Germany</option>
    <option value="IRL">Ireland</option>
    <option value="GBR">United Kingdom</option>
    <option value="USA">United States</option>
  </select>
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
      country_iso: "#country",
    },
  });
```
