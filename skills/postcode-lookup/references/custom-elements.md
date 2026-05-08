# Custom Elements

While Postcode Lookup can reduce much of the friction by rendering postcode lookup elements inside the provided [`context`](https://postcode-lookup.ideal-postcodes.co.uk/classes/Controller.Controller-1.html#context), this is less ideal for forms with complex designs and styling rules. In these instances, you may create one or more custom postcode lookup elements and provide them to Postcode Lookup.

Postcode Lookup consists of 5 main elements with the following order and hierarchy:
- Context
  - Text Input `<input>`
  - Button `<button>`
  - Dropdown Container `<div>`
  - Message Container `<p>`

Identify custom elements to Postcode Lookup by supplying CSS Selectors or node references to your DOM elements. When a custom element is specified, the corresponding element will no longer be rendered in [`context`](https://postcode-lookup.ideal-postcodes.co.uk/classes/Controller.Controller-1.html#context). Listed below are elements of Postcode Lookup which can be supplied to the plugin:

## Postcode Lookup Input Field

[`input`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#input)

Identify an alternate element to use as the Postcode Lookup field.

## Search Button

[`button`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#button)

Identify an alternate element to trigger a postcode lookup.

## Message

[`message`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#message)

Identify an alternate text element to convey messages to the user.

## Address Dropdown Container

[`selectContainer`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#selectContainer)

Identify an alternate element and location to render the address selection dropdown.

## Example

Our WooCommerce plugin takes such an approach to styling the Postcode Lookup UI to conform with the WooCommerce checkout. Our plugin creates an input, button and select container in with the same WooCommerce conventions and leaves the context to manage the message box. [See the plugin code here](https://github.com/ideal-postcodes/woocommerce/blob/master/lib/extension.ts)

## Demo

```html
<form>
    <label>Search your Address</label>
    <input type="text" id="input" placeholder="Enter postcode" />
    <button id="button" type="button">Find Address</button>
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

```css
  #input {
    border-color: #2933e6;
    box-shadow: 0 0 0 1px rgba(41, 51, 230, 0.1);
  }
  #input:focus {
    outline: none;
    border-color: #2933e6;
    box-shadow: 0 0 0 3px rgba(41, 51, 230, 0.2);
  }
  #button {
    margin-top: 0.5rem;
    padding: 0.5rem 1rem;
    font-size: 0.875rem;
    font-weight: 500;
    color: #ffffff;
    background-color: #2933e6;
    border: 1px solid #2933e6;
    border-radius: 0.375rem;
    cursor: pointer;
  }
  #button:hover {
    background-color: #1f29c4;
    border-color: #1f29c4;
  }
  [data-theme="dark"] #input {
    border-color: #545ceb;
    box-shadow: 0 0 0 1px rgba(84, 92, 235, 0.15);
  }
  [data-theme="dark"] #input:focus {
    border-color: #545ceb;
    box-shadow: 0 0 0 3px rgba(84, 92, 235, 0.3);
  }
  [data-theme="dark"] #button {
    background-color: #545ceb;
    border-color: #545ceb;
  }
  [data-theme="dark"] #button:hover {
    background-color: #4047d6;
    border-color: #4047d6;
  }
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
    button: "#button",
    input: "#input",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
