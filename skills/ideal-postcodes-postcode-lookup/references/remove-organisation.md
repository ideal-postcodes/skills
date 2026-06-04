# Omit Company Name

In many countries including the UK, the organisation name will take precedence as the first line of an address if it is present.

Set [`removeOrganisation`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#removeOrganisation) to `true` and include a target `organisation_name` in your `outputFields`. This will strip the address lines of the organisation but separately add the organisation to the input field you have designated with `organisation_name`.

Note that addresses which only have an organisation name as a premise identifier will not have the name stripped.

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
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
    apiKey: "ak_test",
    context: "#lookup_field",
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
