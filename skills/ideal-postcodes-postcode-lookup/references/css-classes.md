# CSS Classes

Postcode Lookup does not inject any styling into your page. Instead, it relies on your pre-existing styles for your forms to render the Postcode Lookup UI.

## CSS Class Hierarchy

Postcode Lookup renders the UI inside of your named `context`. The HTML and CSS classes are structured as shown below:

```html
<!-- Parent Context Element -->
<div id="context">
  <!-- Postcode Lookup Input -->
  <input
    type="text"
    class="idpc-input"
    placeholder="Search your postcode"
    aria-label="Search a postcode to retrieve your address"
    autocomplete="none"
  />
  <!-- Button triggering Postcode Lookup -->
  <button type="button" class="idpc-button">Find my Address</button>
  <!-- Container to render `select` dropdown -->
  <div
    class="idpc-select-container"
    aria-live="polite"
    style="display: none"
  ></div>
  <!-- Any messages displayed here-->
  <p class="idpc-error" role="alert" style="display: none"></p>
</div>
```

Note that:
- The container with class `idpc-select-container` will render the `<select>` element containing address suggestions for a postcode
- The select container is initially hidden
- The message container is initially hidden

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

```css
  .idpc-button {
    background-color: #4a90d9;
    color: white;
    border: none;
    padding: 6px 12px;
    cursor: pointer;
  }
  .idpc-input {
    border: 2px solid #4a90d9;
  }
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    apiKey: "ak_test",
    context: "#context",
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```

## Customise CSS Classes

Custom styling can be achieved by applying your own CSS classes to each HTML element created by Postcode Lookup:

- Search Button `buttonClass`
- Search Input `inputClass`
- Select Container Class `selectContainerClass`
- Select Input `selectClass`
- Message Container `messageClass`
- Unhide Container Class `unhideClass`
