# Default Styling

Address Finder's default styling works by injecting a style tag into the document head.

The default style can be found at [https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder/css/address-finder.css](https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder/css/address-finder.css).

To prevent this, set `injectStyle: false` and load the stylesheet yourself - required under a strict Content Security Policy. See [External Stylesheet](https://docs.ideal-postcodes.co.uk/docs/address-finder/external-stylesheet).

Alternatively, assign a CSS stylesheet URL to `injectStyle` and the widget loads it as a `<link>` tag instead. E.g.

```javascript
{
  injectStyle: "https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder@5/css/address-finder.min.css",
}
```
