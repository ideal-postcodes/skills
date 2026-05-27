# Address Finder

> Add Address Validation to your address forms in moments with our Address Finder JavaScript package. Simple and quick set up. Guides and live technical support available.

<img
  src="https://img.ideal-postcodes.co.uk/address-finder.gif"
  alt="Ideal Postcodes Address Finder Demo"
/>

> 
> Use our [llms.txt](https://docs.ideal-postcodes.co.uk/llms.txt) file with AI tools like ChatGPT, Claude, or Cursor to quickly generate integration code, troubleshoot issues, and get instant answers about Address Finder implementation. Learn more in our [AI Integration guide](https://docs.ideal-postcodes.co.uk/docs/guides/llms).
> 

## Features

- **Rapid Address Entry.** Receive address suggestions as you type with a response time of less than 100ms.
- **Fuzzy Search.** Reduce keystrokes by compensating for spelling mistakes.
- **Word Abbreviations.** Accepts abbreviations such as rd (Road), st (Street) and wy (Way).
- **Transposed Letters.** Handle accidental switching of letters, for instance Liecester (Leicester).
- **Filtering.** Filter suggestions with criteria like locality, country and postcode areas.
- **Biasing.** Bias suggestions towards a location defined by a geospatial point or IP address.
- **Geospatial Filtering.** Restrict suggestions to a geospatial bounding box.
- **Inclusive.** WAI-ARIA compliant and works on screen readers for maximum accessibility.
- **Customisable.** Extensively customisable behaviour and styling.

## Quick Setup

Enable Address Finder by:

1. Adding your API Key with `apiKey`
2. Designating address fields to be autofilled with `outputFields`

> Use the `.watch` method instead of `.setup` if you want to dynamically apply Address Finder when relevant fields appear

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

## How it Works

1. Add the library to your project. Your webpage should have pre-existing address input fields as well as an input field to host the finder.
2. Run initialisation code providing a reference to the Address Finder input and any other configuration.
3. When initialising, Address Finder will perform a key check to determine whether it is usable. If the check fails, initialisation is aborted. Use the `onCheckFailed` callback to update your page for manual address entry.
4. When initialised, Address Finder binds to the input field of your choice and renders a dropdown of address suggestions when the user starts typing.
