# Drop-In Script

Address Finder is shipped in a polyfilled, typed and minified package called `address-finder-bundled`.

`address-finder-bundled` is designed for rapid integration on a webpage or within a bundler.

## Browser Integration

The fastest and simplest way to start is to drop our pre-bundled script onto a webpage.

### Drop-In Script

```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled"></script>
<script>
  IdealPostcodes.AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
</script>
```

`address-finder-bundled` ships a UMD compatible build, which targets IE11 and upwards.

The latest build can be downloaded [here](https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled).

#### Demo

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

<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled@5"></script>
<script>
  IdealPostcodes.AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
</script>
```

### ES Module

`address-finder-bundled` also ships an ESM compatible build, targeting browsers with ESM support and upwards.

```html
<script
  type="module"
  src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled@4/dist/address-finder.esm.js"
></script>

<script type="module">
  import { AddressFinder } from "https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled@4/dist/address-finder.esm.js";
  AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
</script>
```

The latest build can be downloaded [here](https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled/dist/address-finder.esm.js).

#### Demo

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

<script type="module">
  import { AddressFinder } from "https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled@5/dist/address-finder.esm.js";
  AddressFinder.setup({
    apiKey: "ak_test",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
</script>
```

### Version Control

Serving your own versioned copy is recommended. If a JavaScript CDN is used (e.g. jsDelivr, cdnjs), we strongly recommend pinning the version.

It is important you pin your bundle version in production. Pulling directly from latest will cause your integration to fail at some point in the future.

For instance, follow the instructions on [jsdelivr.com/address-finder-bundled](https://www.jsdelivr.com/package/npm/@ideal-postcodes/address-finder-bundled) to pin a major version in production.

E.g.

```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder-bundled"></script>
```

## Bundler Integration

`address-finder-bundled` can also be downloaded via npm and added to your project using bundlers like parcel, webpack or rollup. The advantage of this approach is that it may require less configuration than using the pure `address-finder` module.

### Install

```bash
npm install --save @ideal-postcodes/address-finder-bundled
```

### Use

```javascript
import { AddressFinder } from "@ideal-postcodes/address-finder-bundled";

const controller = AddressFinder.setup({
  apiKey: "ak_test",
  outputFields: {
    line_1: "#line_1",
    line_2: "#line_2",
    line_3: "#line_3",
    post_town: "#post_town",
    postcode: "#postcode",
  },
});
```
