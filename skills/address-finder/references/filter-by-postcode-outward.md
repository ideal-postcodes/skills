# Filter By Postcode Outward

Display address results that matches the chosen postcode outward value.
It will only display matched addresses.

The `queryOptions` object allows customizing the search query. In this example, the postcode_outward property is set to "NW1" to filter the search results towards the NW1 postcode outward code.

### Live Demo

```html
<form>
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
      postcode_outward: "NW1",
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

### Live Demo (Dynamic Postcode Outward)

```html
<form style="max-width: 450px; padding: 10px;">
    <label>Suggest Addresses with Postcode:</label>
    <input type="text" id="filter_postcode" value="SW1A" />
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
      postcode_outward: "sw1a",
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
  const filter = document.getElementById("filter_postcode");
  filter.addEventListener("change", (e) => {
    c.setQueryOptions({
      postcode_outward: e.target.value,
    });
  });
```
