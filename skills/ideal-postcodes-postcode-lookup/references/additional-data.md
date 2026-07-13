# Retrieve Additional Data

We can also pull in more data from the Postcode Address File by passing more arguments into the outputFields object.

In this instance, we pull location, organisation name (if any) and the Unique Property Reference Number.

You can access the full list of [available data points](https://docs.ideal-postcodes.co.uk/docs/api/postcodes).

Try a postcode with lots of local businesses like **NW1 0BG**.

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <div id="lookup_field"></div>
    <!-- Let's add some new input fields below to house the additional data points -->
    <label>Organisation Name</label>
    <input id="organisation_name" type="text" />
    <label>Longitude</label>
    <input id="longitude" type="text" />
    <label>Latitude</label>
    <input id="latitude" type="text" />
    <label>Unique Property Reference Number</label>
    <input id="uprn" type="text" />
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
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#lookup_field",
    removeOrganisation: true,
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
      organisation_name: "#organisation_name",
      longitude: "#longitude",
      latitude: "#latitude",
      uprn: "#uprn",
    },
  });
```
