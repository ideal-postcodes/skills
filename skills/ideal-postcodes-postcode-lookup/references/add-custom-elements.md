# Add Custom Elements

You may have a specific postcode lookup user interface in mind and don't wish to rely on just specifying a context to generate Postcode Lookup elements and CSS to style those elements.

In these instances, you can create your UI with HTML and tell Postcode Lookup to use your elements to drive the address search process.

For example, you can create a postcode lookup interface with Bootstrap and tell Postcode Lookup which elements should form the postcode input field and search button with `input` and `button` elements respectively.

This will also prevent the context from generating its own `input` and `button` elements, leaving the context with just the address `select` element and alerts. So in this example, the context will also serve as its own Bootstrap `form-group`.

The result is a Postcode Lookup which is more closely tailored to your specific framework and user interface goals.

## Live Demo

```html
<form>
    <label for="input">Search your Postcode</label>
    <input type="text" id="input" class="form-control" />
    <div id="lookup_field"></div>
    <button id="button" class="btn btn-dark mt-3">
        Lookup Address
    </button>
    <label for="first_line">Address Line One</label>
    <input type="text" id="first_line" class="form-control" />
    <label for="second_line">Address Line Two</label>
    <input type="text" id="second_line" class="form-control" />
    <label for="third_line">Address Line Three</label>
    <input type="text" id="third_line" class="form-control" />
    <label for="post_town">Post Town</label>
    <input type="text" id="post_town" class="form-control" />
    <label for="postcode">Postcode</label>
    <input type="text" id="postcode" class="form-control" />
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    button: "#button",
    input: "#input",
    selectClass: "form-control",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
