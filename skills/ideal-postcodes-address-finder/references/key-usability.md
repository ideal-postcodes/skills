# Checking the usability of your key

Your key may not be available at some times. For instance:

- Your key may have reached its daily cap set by you
- Your user has reached their individual usage cap for the day
- The balance on your key is exhausted

By default, Address Finder will not initialise if your key is not usable for whatever reason. You may use the `onLoaded` and `onFailedCheck` callbacks to make necessary adjustments.

To disable key checking, set `checkKey` to `false`.

```html
<form style="max-width: 450px; padding: 10px;">
  <p id="message"></p>
  <label for="input">Search Your Address</label>
  <input type="text" id="input" placeholder="Start typing address here" />
  <label for="first_line">Address First Line</label>
  <input type="text" id="first_line" />
  <label for="second_line">Address Second Line</label>
  <input type="text" id="second_line" />
  <label for="third_line">Address Third Line</label>
  <input type="text" id="third_line" />
  <label for="post_town">Town or City</label>
  <input type="text" id="post_town" />
  <label for="postcode">Postcode</label>
  <input type="text" id="postcode" />
</form>
```

```javascript
  import { AddressFinder } from "@ideal-postcodes/address-finder";

AddressFinder.setup({
inputField: "#input",

    // Test key: 'ak_test' passes the check
    apiKey: "ak_test",
    // Test key: 'ak_fails' fails the key check
    // apiKey: 'ak_fails',

    // By default, checkKey is enabled
    checkKey: true,

    // Invoked if plugin loads
    onLoaded: function () {
      document.getElementById("message").textContent =
        "Check passed!";
    },

    // Invoked if key fails check
    onFailedCheck: function () {
      document.getElementById("message").textContent =
        "Check failed. Please manually enter your address";
    },

    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },

});
```
