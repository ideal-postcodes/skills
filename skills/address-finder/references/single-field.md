# Display Address in One Line

Instead of writing each address attributes to individual fields, you may exercise fine control about how address data is presented using the [`onAddressRetrieved`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onAddressRetrieved) callback.

In this example, we merge the addresses and insert it to a `textarea`.

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
