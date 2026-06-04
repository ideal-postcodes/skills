# Run Custom Callback

By passing a function to a callback like [`onSearchCompleted`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#onSearchCompleted), you can modify or extend the behaviour of Postcode Lookup.

This option is useful if you want to run custom code after the user has searched for an address and before the user selects from the dropdown menu.

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <pre id="successStatus"></pre>
    <div id="lookup_field"></div>
    <div id="address_form">
        <label>Organisation</label>
        <input id="organisation" type="text" />
        <label>Address Line One</label>
        <input id="first_line" type="text" />
        <label>Address Line Two</label>
        <input id="second_line" type="text" />
        <label>Address Line Three</label>
        <input id="third_line" type="text" />
        <label>Post Town</label>
        <input id="post_town" type="text" />
        <label>Postcode</label>
        <input id="postcode" type="text" />
    </div>
</form>
```

```javascript
  import { PostcodeLookup } from "@ideal-postcodes/postcode-lookup";

  PostcodeLookup.setup({
    context: "#lookup_field",
    apiKey: "ak_test",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
    // onSearchCompleted is invoked with a data object representing the JSON body of the request.
    // You should check the code data.code to observe the outcome of the request.
    // 2000 means success. 4040 means postcode does not exist.
    // Other codes will mean an error occurred.
    onSearchCompleted: function (error, addresses) {
      if (error) {
        document.getElementById("successStatus").textContent =
          "Some error occurred";
      }
      // Message dependent on postcode existence
      if (addresses.length === 0) {
        document.getElementById("successStatus").textContent =
          "Postcode does not exist";
      } else {
        document.getElementById("successStatus").textContent = "Success!";
      }
    },
  });
```
