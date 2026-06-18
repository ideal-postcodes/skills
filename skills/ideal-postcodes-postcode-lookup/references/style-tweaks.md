# Style Adjustments

It's possible that the default styling may require a small tweak to allow it to better match your design and site aesthetic. Rather than adding CSS assets, consider applying a style attribute as a setup configuration option. The configurations below apply a style attribute directly to a specific element of the Postcode Lookup.

## Input

`inputStyle`

Applies additional styling to the postcode lookup field.

## Container Element

`contextStyle`

Applies styling to the Postcode Lookup UI container element.

## Button Style

`buttonStyle`

Applies additional styling to the search button.

## Message Style

`messageStyle`

Applies styling to the message container.

## Address Dropdown Style

`selectStyle`

Applies styling to the `<select>` element displaying address selections.

## Demo

```html
<form>
    <label>Search your Address</label>
    <div id="context"></div>
    <label for="line_1">Address Line One</label>
    <input type="text" id="line_1" />
    <label for="line_2">Address Line Two</label>
    <input type="text" id="line_2" />
    <label for="line_3">Address Line Three</label>
    <input type="text" id="line_3" />
    <label for="post_town">Post Town</label>
    <input type="text" id="post_town" />
    <label for="postcode">Postcode</label>
    <input type="text" id="postcode" />
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
    inputStyle: {
      borderColor: "#16a34a",
    },
    buttonStyle: {
      fontSize: "1rem",
      fontWeight: "600",
      backgroundColor: "#16a34a",
      borderColor: "#16a34a",
    },
    selectStyle: {
      borderColor: "#16a34a",
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
