# Retrieve Additional Data

You can retrieve more data for an address by passing more arguments into `outputFields`.

The key value should map to an address field described in our [standard address schema](https://docs.ideal-postcodes.co.uk/docs/data/paf).

## Example

In this example, geolocation, organisation name and the Unique Property Reference Number are inserted into additional input fields.

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
  <!-- Additional Fields -->
  <label for="organisation_name">Organisation Name</label>
  <input type="text" id="organisation_name" />
  <label for="uprn">Unique Property Reference Number</label>
  <input type="text" id="uprn" />
  <label for="longitude">Longitude</label>
  <input type="text" id="longitude" />
  <label for="latitude">Latitude</label>
  <input type="text" id="latitude" />
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
      // Organisation
      organisation_name: "#organisation_name",
      // Unique Property Reference Number
      uprn: "#uprn",
      // Geolocation
      longitude: "#longitude",
      latitude: "#latitude",
    },
  });
```
