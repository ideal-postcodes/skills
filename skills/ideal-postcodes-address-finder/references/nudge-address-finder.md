# Vertically Shift Address Finder

From time to time, you may want to tweak the default style of Address Finder to better match your application's design system.

[JavaScript Style Adjustments](https://docs.ideal-postcodes.co.uk/docs/address-finder/style-js) are the recommended means to make small changes.

A common issue is the alignment of the Address Finder dropdown, this may be a few pixels too high or low. In these instances, we recommend a positive or negative `margin-top` style attribute.

## Live Demo

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
    listStyle: {
      marginTop: "-1.4rem",
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
