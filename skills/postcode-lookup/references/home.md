# Postcode Lookup

<img src="https://img.ideal-postcodes.co.uk/postcode-lookup.gif" alt="Ideal Postcodes Postcode Lookup" />

> 
> Use our [llms.txt](https://docs.ideal-postcodes.co.uk/llms.txt) file with AI tools like ChatGPT, Claude, or Cursor to quickly generate integration code, troubleshoot issues, and get instant answers about Postcode Lookup implementation. Learn more in our [AI Integration guide](https://docs.ideal-postcodes.co.uk/docs/guides/llms).
> 

## Features

- **Rapid Address Retrieval.** Postcode Lookup is the most widely understood and fastest way to retrieve a UK address.
- **Fuzzy Matching.** Suggests nearest matching postcodes if invalid postcode provided.
- **Postcode Fixing.** Fixes common mistakes in postcodes.
- **Inclusive.** WAI-ARIA compliant and works on screen readers for maximum accessibility.
- **Customisable.** Extensively customisable behaviour and styling.

## Quick Setup

Enable Postcode Lookup by:
1. Adding your API Key with `apiKey`
2. Provide an area to render our UI with `context`
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
