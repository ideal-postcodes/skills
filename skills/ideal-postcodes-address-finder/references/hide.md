# Hide Address Fields

## Hide

In some instances, you may want to hide the address inputs and subsequently unhide them when the user has selected a verified address. This makes it harder for the user to input an incorrect address and bypass address verification.

Address Finder provides this functionality natively using `hide`. Specify which HTML Elements you would like to hide as CSS Selector or references. When the user selects an address, or address verification fails (no balance, limit reached, etc), the fields will unhide.

A clickable link will also be provided to allow the user to manually enter an address.

## Custom Unhide Element

You can assign a custom element to serve as a clickable element to unhide the address fields. Use the `unHide` option.

## Live Demo

```html
<form style="max-width: 450px; padding: 10px;">
    <label for="input">Search Your Address</label>
    <input type="text" id="input" placeholder="Start typing address here" />
    <div id="hiddenFields">
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
    </div>
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  AddressFinder.setup({
    apiKey: "ak_test",
    hide: ["#hiddenFields"],
    inputField: "#input",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
