# Style with JavaScript

It's possible that the default styling may require a small tweak to allow it to better match your design and site aesthetic. Rather than adding CSS assets, we recommend applying a style attribute as a setup configuration option. The configurations below apply a style attribute directly to a specific element of the address finder.

## Address Finder Container

[`containerStyle`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#ContainerStyle)

Applies additional styling to the entire Address Finder including the input field.

## Input Style

[`inputStyle`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#InputStyle)

Applies additional styling to the main input field that triggers Address Finder.

## Main Style

[`mainStyle`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#MainStyle)

Applies styling to the toggleable Address Finder (includes suggestion list and toolbar).

## Suggestion List

[`listStyle`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#ListStyle)

Applies additional styling to the suggestion list.

## Suggestion Element

[`liStyle`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#LiStyle)

Applies custom styling to every list element `<li>` including address suggestions and messages.

## Demo

This sandbox provides a visual demonstration of how a style attribute impacts the overall styling of Address Finder.

```html
<form style="max-width: 450px; padding: 10px;">
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
    // Encapsulates everything (including input)
    containerStyle: {
      fontSize: "20px",
    },
    // Modifies the input field
    inputStyle: {
      backgroundColor: "orange",
    },
    // Encapsulates toggleable Address Finder tool
    mainStyle: {
      border: "2px solid red",
    },
    // Main list element
    listStyle: {
      backgroundColor: "cyan",
    },
    // Styles each suggestion item
    liStyle: {
      border: "1px solid green",
    },
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
