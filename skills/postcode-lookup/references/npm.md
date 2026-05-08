# Package Manager (npm)

Postcode Lookup is also available without transpilation or minification on npm as [`@ideal-postcodes/postcode-lookup`](https://www.npmjs.com/package/@ideal-postcodes/postcode-lookup).

The `postcode-lookup` npm package works out-of-the-box with most bundlers. The advantage of consuming this package are:
- Finer control over which browsers you wish to support
- Dependency deduplification
- Tree shaking
- Smaller package sizes

[`@ideal-postcodes/postcode-lookup-bundled`](https://www.npmjs.com/package/@ideal-postcodes/postcode-lookup-bundled) also works with bundlers. `postcode-lookup-bundled` will work in all bundlers with no configuration changes as it is conservatively pre-transpiled, polyfilled and minified.

> Use `postcode-lookup-bundled` if you are having difficulty adding `postcode-lookup` to your bundler. The output may be slightly larger but it is guaranteed to work without changing your bundler configuration.

### Add

Add postcode-lookup to your project via npm with

```bash
npm install @ideal-postcodes/postcode-lookup
```

### Instantiate

Instantiate Postcode Lookup with `PostcodeLookup.setup`.

```js
import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

const controller = PostcodeLookup.setup({
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
