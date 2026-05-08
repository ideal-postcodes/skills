# Create Multiple Instances

You can create multiple, independent Postcode Lookup fields on the same page. This is useful for certain requirements, e.g. separate billing and shipping addresses.

[`setup`](https://postcode-lookup.ideal-postcodes.co.uk/modules/Setup.html#setup) can be invoked multiple times on different DOM elements to create multiple fields.

Each lookup field is independent and so can behave differently if you pass in different configuration settings.

## Live Demo

```html
<form>
    <div class="box">
        <h2>Lookup Field 1</h2>
        <label>Search your Address</label>
        <div id="lookup_field"></div>
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
    <div class="box">
        <h2>Lookup Field 2</h2>
        <div id="lookup_field_2"></div>
        <label>Address Line One</label>
        <input id="first_line_2" type="text" />
        <label>Address Line Two</label>
        <input id="second_line_2" type="text" />
        <label>Address Line Three</label>
        <input id="third_line_2" type="text" />
        <label>Post Town</label>
        <input id="post_town_2" type="text" />
        <label>Postcode</label>
        <input id="postcode_2" type="text" />
    </div>
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  // Initialize each lookup field individually. You can specify completely different configurations. Each is isolated from the other.
  // Hook up the first lookup field
  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
  // Hookup the second lookup field
  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field_2",
    outputFields: {
      line_1: "#first_line_2",
      line_2: "#second_line_2",
      line_3: "#third_line_2",
      post_town: "#post_town_2",
      postcode: "#postcode_2",
    },
  });
```
