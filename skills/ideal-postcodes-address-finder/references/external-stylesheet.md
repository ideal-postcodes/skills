# Disable style injection and load the CSS externally

Address Finder applies its [default styling](https://docs.ideal-postcodes.co.uk/docs/address-finder/default-styling) by injecting an inline `<style>` tag into the document head. A strict Content Security Policy (one whose `style-src` omits `'unsafe-inline'`) blocks that tag and the widget renders unstyled.

The same styles ship as static files in the npm package (`css/address-finder.css` and `css/address-finder.min.css`), also served by jsDelivr. Disable injection with `injectStyle: false` and load the stylesheet like any other.

## Link the stylesheet

Add a `<link>` tag, pinning at least the major version to match your installed widget:

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder@5/css/address-finder.min.css"
/>
```

Then disable injection:

```javascript
AddressFinder.setup({
  apiKey: "ak_test",
  injectStyle: false,
  outputFields: {
    line_1: "#line_1",
    line_2: "#line_2",
    post_town: "#post_town",
    postcode: "#postcode",
  },
});
```

Your CSP needs to allow the stylesheet host, e.g. `style-src 'self' https://cdn.jsdelivr.net`.

## Point `injectStyle` at a URL

`injectStyle` also accepts a stylesheet URL. The widget then appends a `<link rel="stylesheet">` element, not an inline `<style>` tag, so this works under the same CSP as a hand-written link:

```javascript
AddressFinder.setup({
  apiKey: "ak_test",
  injectStyle:
    "https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder@5/css/address-finder.min.css",
});
```

## Bundle the CSS yourself

If you install via npm and use a bundler, import the stylesheet into your build and set `injectStyle: false`:

```javascript
import "@ideal-postcodes/address-finder/css/address-finder.min.css";
```

The unminified `@ideal-postcodes/address-finder/css/address-finder.css` is also published if you want to compose or override rules in your own pipeline. React users should import from `@ideal-postcodes/react` instead - see [React CSS](https://docs.ideal-postcodes.co.uk/docs/address-finder/react#css).

## Live Demo

```html
<link
  rel="stylesheet"
  href="https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder@5/css/address-finder.min.css"
/>
<form style="max-width: 450px; padding: 10px;">
  <label for="line_1">Address First Line</label>
  <input type="text" id="line_1" />
  <label for="line_2">Address Second Line</label>
  <input type="text" id="line_2" />
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
    injectStyle: false,
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
