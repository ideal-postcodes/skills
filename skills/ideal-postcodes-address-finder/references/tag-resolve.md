# Tag Resolve Requests

Tag your address resolve requests with metadata for usage tracking. Tags are appended to the resolve API call (the second request that retrieves the full address when a suggestion is selected).

See [Tagging and Usage Tracking](https://docs.ideal-postcodes.co.uk/docs/guides/tag-and-track) for more on how tags work.

#### JavaScript

```javascript
AddressFinder.setup({
    apiKey: "ak_test",
    resolveOptions: {
        tags: "checkout,web"
    },
    outputFields: {
        line_1: "#line_1",
        line_2: "#line_2",
        line_3: "#line_3",
        post_town: "#post_town",
        postcode: "#postcode"
    }
});
```

#### HTML

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="line_1">Address First Line</label>
    <input type="text" id="line_1" />
    <label for="line_2">Address Second Line</label>
    <input type="text" id="line_2" />
    <label for="line_3">Address Third Line</label>
    <input type="text" id="line_3" />
    <label for="post_town">Town or City</label>
    <input type="text" id="post_town" />
    <label for="postcode">Postcode</label>
    <input type="text" id="postcode" />
</form>
```

## Dynamic Tags

Use `setResolveOptions` to update tags after the controller has been instantiated. This is useful when the tag value depends on user context, such as the current page or form.

#### JavaScript

```javascript
const c = AddressFinder.setup({
    apiKey: "ak_test",
    resolveOptions: {
        tags: "default"
    },
    outputFields: {
        line_1: "#line_1",
        line_2: "#line_2",
        line_3: "#line_3",
        post_town: "#post_town",
        postcode: "#postcode"
    }
});

// Update tags based on which form is active
document.getElementById("form_select").addEventListener("change", (e) => {
    c.setResolveOptions({
        tags: e.target.value
    });
});
```

#### HTML

```html
<form style="max-width: 450px; padding: 10px;">
    <label>Tag:</label>
    <select id="form_select">
        <option value="checkout">Checkout</option>
        <option value="registration">Registration</option>
    </select>
    <label for="line_1">Address First Line</label>
    <input type="text" id="line_1" />
    <label for="line_2">Address Second Line</label>
    <input type="text" id="line_2" />
    <label for="line_3">Address Third Line</label>
    <input type="text" id="line_3" />
    <label for="post_town">Town or City</label>
    <input type="text" id="post_town" />
    <label for="postcode">Postcode</label>
    <input type="text" id="postcode" />
</form>
```
