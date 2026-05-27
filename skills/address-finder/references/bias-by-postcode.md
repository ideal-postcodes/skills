# Bias By Postcode

Display address results that closely match the chosen postcode value.

It will still display unmatched addresses but they will be ranked lower.

The `queryOptions` object allows customizing the search query. In this example, the `bias_postcode` property is set to "BR8 7RE" to bias the search results towards the BR8 7RE postcode.

#### Live Demo (Static Postcode)

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

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    queryOptions: {
      bias_postcode: "BR8 7RE",
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```

## Dynamically Updating Bias

The example below includes an event listener, which allows users to dynamically change the bias. When the value of the input field changes, `setQueryOptions` function is used to apply the new bias.

#### Live Demo (Dynamic Postcode)

```html
<form style="max-width: 450px; padding: 10px;">
    <label>Suggest Addresses with Postcode:</label>
    <input type="text" id="bias_postcode" value="BR8 7RE" />
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

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  const c = AddressFinder.setup({
    apiKey: "ak_test",
    queryOptions: {
      bias_postcode: "BR8 7RE",
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
  const bias = document.getElementById("bias_postcode");
  bias.addEventListener("change", (e) => {
    c.setQueryOptions({
      bias_postcode: e.target.value,
    });
  });
```
