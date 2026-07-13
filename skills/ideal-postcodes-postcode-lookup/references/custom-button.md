# Custom Search Button

You may also specify any clickable DOM element as a trigger to look up a postcode.

Identify your custom button via the `button` property.

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
    <!-- Here we've created a link to be used as a trigger for a postcode lookup -->
    <h3><a href="" id="customButton">My Custom Lookup Link</a></h3>
    <div id="address_form">
        <label>Organisation</label>
        <input id="organisation" type="text" />
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
    context: "#lookup_field",
    apiKey: "ak_test",
    button: "#customButton",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
