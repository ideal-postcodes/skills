# Package Manager (npm)

Address Finder is also available without transpilation or minification on npm as [`@ideal-postcodes/address-finder`](https://www.npmjs.com/package/@ideal-postcodes/address-finder).

The `address-finder` npm package works out-of-the-box with most bundlers. The advantages of consuming this package are:

- Finer control over which browsers you wish to support
- Dependency deduplication
- Tree shaking
- Smaller package sizes

[`@ideal-postcodes/address-finder-bundled`](https://www.npmjs.com/package/@ideal-postcodes/address-finder-bundled) also works with bundlers. `address-finder-bundled` will work in all bundlers with no configuration changes as it is conservatively pre-transpiled, polyfilled and minified.

> Use `address-finder-bundled` if you are having difficulty adding `address-finder` to your bundler. The output may be slightly larger but it is guaranteed to work without changing your bundler configuration.

### Add

Add address-finder to your project via npm with

```bash
npm install @ideal-postcodes/address-finder
```

### Instantiate

Instantiate Address Finder with `AddressFinder.setup`.

```js
import { AddressFinder } from "@ideal-postcodes/address-finder";

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

### Demo

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
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
