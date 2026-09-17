# Postcode Lookup

<img src="https://img.ideal-postcodes.co.uk/postcode-lookup.gif" alt="Ideal Postcodes Postcode Lookup" />

## Features

- **Rapid Address Retrieval.** Postcode Lookup is the most widely understood and fastest way to retrieve a UK address.
- **Fuzzy Matching.** Suggests nearest matching postcodes if an invalid postcode is provided.
- **Postcode Fixing.** Fixes common mistakes in postcodes.
- **Inclusive.** Works on screen readers and is fully keyboard operable, with results presented in a native select. See [WAI-ARIA support](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/messages#wai-aria).
- **Customisable.** Extensively customisable behaviour and styling.

## Quick Setup

Enable Postcode Lookup by:
1. Adding your API Key with `apiKey`
2. Providing an area to render our UI with `context`
3. Designating address fields to be autofilled with `outputFields`

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
    <label for="first_line">Address Line One</label>
    <input type="text" id="first_line" />
    <label for="second_line">Address Line Two</label>
    <input type="text" id="second_line" />
    <label for="third_line">Address Line Three</label>
    <input type="text" id="third_line" />
    <label for="post_town">Post Town</label>
    <input type="text" id="post_town" />
    <label for="postcode">Postcode</label>
    <input type="text" id="postcode" />
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

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
```
