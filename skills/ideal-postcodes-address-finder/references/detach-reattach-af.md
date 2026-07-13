# Detach and Re-attach Address Finder on your form

### Detaching Address Finder

When you want to remove Address Finder from your form (e.g. after the user selects an address), you can do so by calling `.detach()` on the Address Finder controller. This will disable Address Finder's listening capabilities and remove any address suggestions.

For instance, you can call `this.detach()` in the onAddressPopulated callback function to stop the Address Finder after the user has selected an address.

### Re-Attaching Address Finder

You can attach Address Finder back to line one if you hold a reference to the controller with `.attach()`.

### Live Demo

```html
<style>
  .reset-btn {
    margin-top: 1rem;
    padding: 0.5rem 1rem;
    font-size: 0.875rem;
    font-weight: 500;
    color: #ffffff;
    background-color: #2933e6;
    border: none;
    border-radius: 0.375rem;
    cursor: pointer;
    transition: background-color 150ms ease;
  }
  .reset-btn:hover { background-color: #1f28b8; }
</style>
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
    <button class="reset-btn" type="button" onclick="reAttach()">Reset Form</button>
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

  const controller = AddressFinder.setup({
    apiKey: "ak_test",
    onAddressPopulated: function () {
      this.detach();
    },
    outputFields: {
      line_1: "#line_1",
      line_2: "#line_2",
      line_3: "#line_3",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });

  window.reAttach = function () {
    controller.attach();
  };
```
