# Default Styling

Address Finder's default styling works by injecting a style tag into the document head.

The default style can be found at [https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder/css/address-finder.css](https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder/css/address-finder.css).

To prevent this, set `injectStyle`<br/>

To provide your own CSS stylesheet you can assign a CSS Stylesheet link to `injectStyle`. E.g.

```javascript
{
  injectStyle: "https://cdn.jsdelivr.net/npm/@ideal-postcodes/address-finder@1.2.1/css/address-finder.min.css",
}
```
