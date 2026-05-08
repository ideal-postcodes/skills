# Multi-page Form

You can have the user enter their postcode and open the address form  in a new page by saving the user's input in [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage).

This simulates a user entering a postcode when the Postcode Lookup API is initialised.

## Postcode Page

The first page collects the postcode and stores it in `localStorage`:

```js
localStorage.clear();
const postcode = document.getElementById("postcode");
const btn = document.getElementById("btn-postcode");
btn.addEventListener("click", function () {
  localStorage.setItem("postcode", postcode.value);
});
```

```html
<form>
  <label for="postcode">Type Postcode Below</label>
  <input type="text" id="postcode" />
  <a href="/form.html">
    <button id="btn-postcode">Search Postcode</button>
  </a>
  <p>
    The postcode you search will be available on the next page using
    localStorage
  </p>
</form>
```

## Form Page

The address form page reads the stored postcode and auto-triggers the lookup on load:

## Live Demo

```html
<form>
    <label>Search your Address</label>
    <div id="context"></div>
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

  const pl = PostcodeLookup.setup({
    onLoaded: () => {
      pl.input.value = localStorage.getItem("postcode");
      pl.button.click();
    },
    context: "#context",
    apiKey: "ak_test",
    outputFields: {
      line_1: "#first_line",
      line_2: "#second_line",
      line_3: "#third_line",
      post_town: "#post_town",
      postcode: "#postcode",
    },
  });
```
