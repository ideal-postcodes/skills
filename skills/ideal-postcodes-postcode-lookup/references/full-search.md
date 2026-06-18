# Enable Full Address Search

You can also enable a full address search, i.e. combine a postcode with house number.

Set `strictlyPostcodes` to `false`.

## Live Demo

```html
<form>
    <div class="box">
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
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    // Enable address search
    strictlyPostcodes: false,
    // Add a custom label
    placeholder: "Search for a postcode or address",
    // Populate the address if a single match found
    selectSinglePremise: true,
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
