# Attach Multiple Instances

Address Finder can be invoked multiple times to attach multiple instances to your forms.

Each Address Finder instance is isolated and can be configured individually.

The example below illustrates how this can be done two invocations of `AddressFinder.setup()`. Each with a unique `inputField` property. This allows the user to search for an address using either the first or second line of the address.

## Live Demo

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="first_line">Address First Line</label>
    <input type="text" id="first_line" />
    <label for="second_line">Address Second Line</label>
    <input type="text" id="second_line" />
    <label for="third_line">Address Third Line</label>
    <input type="text" id="third_line" />
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
    inputField: "#first_line",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
  AddressFinder.setup({
    apiKey: "ak_test",
    inputField: "#second_line",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
