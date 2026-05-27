# CSS Classes

The Address Finder can be styled with CSS classes.

## CSS Class Hierarchy

```html
<div class="idpc_autocomplete">
  <input type="text" />
  <div class="idpc_af">
    <ul class="idpc_ul">
      <li class="idpc_error">
        This element only shows when a message is communicated to the user
      </li>
      <li aria-selected="true">Address Suggestion One</li>
      <li aria-selected="false">Address Suggestion Two</li>
      <li aria-selected="false">Address Suggestion Three</li>
    </ul>
    <div class="idpc_toolbar">
      <span class="idpc_country">
        <span class="idpc_icon">🇮🇪</span>
        <span class="idpc_country">Select Country</span>
      </span>
    </div>
  </div>
</div>
```

## Demo

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

```css
  .idpc_autocomplete {
    border: 2px solid #2933e6;
    border-radius: 0.375rem;
  }
  
  .idpc_af {
    background-color: #f9fafb;
    border: 1px solid #d1d5db;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
  
  .idpc_af > ul > li {
    padding: 0.625rem 0.75rem;
    color: #111827;
    cursor: pointer;
  }
  
  .idpc_af > ul > li:hover {
    background-color: #e0e7ff;
  }
  
  .idpc_af > ul > li[aria-selected="true"] {
    background-color: #2933e6;
    color: #ffffff;
  }
  
  .idpc_error {
    background-color: #fee2e2 !important;
    color: #991b1b !important;
  }
  
  .idpc_toolbar {
    border-top: 1px solid #e5e7eb;
    padding: 0.5rem;
  }
  
  .idpc_country {
    color: #6b7280;
    font-size: 0.875rem;
  }
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
