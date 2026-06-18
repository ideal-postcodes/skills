# Merge Address into One Field

Instead of writing address attributes to specific input fields, you can also write out the entire address to a general input like a `textarea`.

This is achieved by using the `onAddressSelected` callback.

Postcode Lookup provides a number of [callbacks](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks) to enable custom behaviours.

## Live Demo

```html
<form>
    <div class="box">
        <label>Search your Address</label>
        <div id="lookup_field"></div>
        <label>Address</label>
        <textarea id="output_field"></textarea>
    </div>
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    onAddressSelected: function (address) {
      const result = [
        address.line_1,
        address.line_2,
        address.line_3,
        address.post_town,
        address.postcode,
      ]
        .filter((elem) => elem !== "")
        .join(", ");
      document.getElementById("output_field").textContent = result;
    },
  });
```
