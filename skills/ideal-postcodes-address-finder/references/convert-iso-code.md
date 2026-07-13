# Convert ISO-3 to ISO-2

By default, the Ideal Postcodes API returns the country code in ISO-3 format.
If your platform takes in ISO-2 codes instead, use the `onLoaded` callback function to remap the ISO-3 codes to ISO-2.

## Live Demo

```html
<form style="max-width: 450px; padding: 10px;">
  <label for="countries">Choose a Country:</label>
  <select id="countries" name="country">
    <option value="">Select a country</option>
    <option value="AL">Albania</option>
    <option value="AD">Andorra</option>
    <option value="AT">Austria</option>
    <option value="BY">Belarus</option>
    <option value="BE">Belgium</option>
    <option value="BA">Bosnia and Herzegovina</option>
    <option value="BG">Bulgaria</option>
    <option value="HR">Croatia</option>
    <option value="CY">Cyprus</option>
    <option value="CZ">Czech Republic</option>
    <option value="DK">Denmark</option>
    <option value="EE">Estonia</option>
    <option value="FI">Finland</option>
    <option value="FR">France</option>
    <option value="DE">Germany</option>
    <option value="GR">Greece</option>
    <option value="HU">Hungary</option>
    <option value="IS">Iceland</option>
    <option value="IE">Ireland</option>
    <option value="IT">Italy</option>
    <option value="LV">Latvia</option>
    <option value="LI">Liechtenstein</option>
    <option value="LT">Lithuania</option>
    <option value="LU">Luxembourg</option>
    <option value="MK">North Macedonia</option>
    <option value="MT">Malta</option>
    <option value="MD">Moldova</option>
    <option value="MC">Monaco</option>
    <option value="ME">Montenegro</option>
    <option value="NL">Netherlands</option>
    <option value="NO">Norway</option>
    <option value="PL">Poland</option>
    <option value="PT">Portugal</option>
    <option value="RO">Romania</option>
    <option value="RU">Russia</option>
    <option value="SM">San Marino</option>
    <option value="RS">Serbia</option>
    <option value="SK">Slovakia</option>
    <option value="SI">Slovenia</option>
    <option value="ES">Spain</option>
    <option value="SE">Sweden</option>
    <option value="CH">Switzerland</option>
    <option value="UA">Ukraine</option>
    <option value="GB">United Kingdom</option>
    <option value="VA">Vatican City</option>
  </select>
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
    onLoaded: function () {
      const dropdown = document.getElementById("countries");
      const c = this;
      // Add an event listener to listen for changes in the selected option
      dropdown.addEventListener("change", function () {
        // Get the selected value
        const iso_2 = dropdown.value;
        // Extract ISO3 if available
        const contexts = Object.values(c.options.contexts);
        for (const context of contexts) {
          if (context.iso_2 === iso_2) {
            c.applyContext(context);
            break;
          }
        }
      });
    },
  });
```
