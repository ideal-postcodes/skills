# Display Address in One Line

Instead of writing each address attribute to individual fields, you may exercise fine control over how address data is presented using the `onAddressRetrieved` callback.

In this example, we merge the address lines and insert them into a `textarea`.

## Live Demo

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="input">Search Your Address</label>
    <input type="text" id="input" placeholder="Start typing address here" />
    <label for="output_field">Address Output:</label>
    <pre id="output_field"></pre>
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    inputField: "#input",
    onAddressRetrieved: (address) => {
      const result = [
        address.line_1,
        address.line_2,
        address.line_3,
        address.post_town,
        address.postcode,
      ]
        .filter((elem) => elem !== "")
        .join("\\n");
      document.getElementById("output_field").textContent = result;
    },
  });
```
