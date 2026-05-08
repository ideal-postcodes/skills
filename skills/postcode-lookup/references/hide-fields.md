# Hide Address Fields

Instead of writing address attributes to specific input fields, you can also write out the entire address to a general input like a `textarea`.

This is achieved by using the [`onAddressSelected`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#onAddressSelected) callback.

Postcode Lookup provides a number of [callbacks](callbacks) to enable custom behaviours.

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
