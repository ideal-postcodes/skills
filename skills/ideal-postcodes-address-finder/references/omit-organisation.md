# Omit Company Name

In many countries including the UK, the organisation name will take precedence as the first line of an address if it is present.

By setting [`removeOrganisation`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#RemoveOrganisation) to true, the plugin will scrub any organisation name from address lines.

> Note that in instances where the organisation name is the _only_ premise identifier, it will not be removed from an address.

## Live Demo

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="input">Search Your Address</label>
    <input type="text" id="input" placeholder="Start typing address here" />
    <hr style="margin: 1rem 0; border: none; border-top: 1px solid #e5e7eb;" />
    <label for="organisation">Organisation Name</label>
    <input type="text" id="organisation" />
    <label for="first_line">Address First Line</label>
    <input type="text" id="first_line" />
    <label for="second_line">Address Second Line</label>
    <input type="text" id="second_line" />
    <label for="third_line">Address Third Line</label>
    <input type="text" id="third_line" />
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
    inputField: "#input",
    removeOrganisation: true,
    outputFields: {
      organisation_name: "#organisation",
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
