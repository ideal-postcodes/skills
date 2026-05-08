# Drop-In Script

Postcode Lookup shipped in a polyfilled, typed and minified package called `postcode-lookup-bundled`.

`postcode-lookup-bundled` is designed for rapid integration on a webpage or within a bundler.

## Browser Integration

The fastest and simplest way to start is to drop our pre-bundled script onto a webpage.

### Drop-In Script

```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3"></script>
<script>
  IdealPostcodes.PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
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

`postcode-lookup-bundled` ships a UMD compatible build, which targets IE11 and upwards.

The latest build can be downloaded [here](https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled).

#### Demo

```html
<form>
  <label>Search your Address</label>
  <div id="context"></div>
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

<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3"></script>
<script>
  IdealPostcodes.PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
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

`postcode-lookup-bundled` also ships an ESM compatible build, targeting browsers with ESM support and upwards.

```html
<script
  type="module"
  src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3/dist/postcode-lookup.esm.js"></script>

<script type="module">
  import { PostcodeLookup } from "https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3/dist/postcode-lookup.esm.js";
  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
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

The latest build can be downloaded [here](https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled/dist/postcode-lookup.esm.js).

#### Demo

```html
<form>
  <label>Search your Address</label>
  <div id="context"></div>
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
  import { PostcodeLookup } from "https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3/dist/postcode-lookup.esm.js";
  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
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

For instance, follow the instructions on [jsdelivr.com/postcode-lookup-bundled](https://www.jsdelivr.com/package/npm/@ideal-postcodes/postcode-lookup-bundled) to pin a major version in production.

E.g.
```html
<script src="https://cdn.jsdelivr.net/npm/@ideal-postcodes/postcode-lookup-bundled@3"></script>
```

## Bundler Integration

`postcode-lookup-bundled` can also be downloaded via npm and added to your project using bundlers like parcel, webpack or rollup. The advantage of this approach is that it may require less configuration than using the pure `postcode-lookup` module.

### Install

```bash
npm install --save @ideal-postcodes/postcode-lookup-bundled
```

### Use

```javascript
import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup-bundled";

const controller = PostcodeLookup.setup({
  apiKey: "ak_test",
  context: "#context",
  outputFields: {
    line_1: "#line_1",
    line_2: "#line_2",
    line_3: "#line_3",
    post_town: "#post_town",
    postcode: "#postcode",
  },
});
```
