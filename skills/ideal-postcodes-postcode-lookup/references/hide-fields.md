# Hide Address Fields

You can hide your address input fields until the user has selected a verified address, making it harder to bypass address verification with an unrecognised entry.

Pass the elements to conceal to the `hide` option as CSS selectors or direct DOM references. Postcode Lookup hides them on initialisation and unhides them once an address is selected or verification fails. A clickable link is also rendered so the user can choose to enter an address manually.

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
    <!-- The above empty div tag will allow the plugin to create the lookup fields -->
    <div id="address_form">
        <label>Address Line One</label>
        <input id="first_line" type="text" />
        <label>Address Line Two</label>
        <input id="second_line" type="text" />
        <label>Address Line Three</label>
        <input id="third_line" type="text" />
        <label>Post Town</label>
        <input id="post_town" type="text" />
        <label>Postcode</label>
        <input id="postcode" type="text" />
    </div>
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    hide: ["#address_form"],
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
