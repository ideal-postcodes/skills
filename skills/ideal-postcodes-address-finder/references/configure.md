# Configure

Address Finder exports a [`setup`](https://address-finder.ideal-postcodes.co.uk/modules/Controller.html#Setup) method to apply address verification to a form. [`setup`](https://address-finder.ideal-postcodes.co.uk/modules/Controller.html#Setup) requires the following configuration at minimum.

> `apiKey` and `outputFields` below are all you need to get Address Finder working. For the complete set of controller options, see [Additional Configuration](https://docs.ideal-postcodes.co.uk/docs/address-finder/additional-configuration).

## API Key

[`apiKey`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#ApiKey)

API Key from your Ideal Postcodes account. Typically begins `ak_`

## Address Targets

[`outputFields`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#OutputFields)

Specify where to send address data given a selected address. `outputFields` is an object which maps an address attribute to an input field. The input field can be identified by CSS or reference to the DOM element itself.

```javascript
{
  line_1: "#line_1",
  line_2: "#line_2",
  line_3: "input[name='line_3']",
  post_town: document.getElementById("post_town"),
  postcode: document.getElementById("postcode")
}
```

> The fields above (`line_1`, `line_2`, `line_3`, `post_town` and `postcode`) are the minimum set needed to capture a complete, deliverable UK address. Collect all of them so the address you store reliably routes to the premises.

Assigning up to 3 address lines, post town and postcode fields, is all the addressing information required to identify a UK premises. You may extract more data for an address by passing more properties into the `outputFields` configuration object.

The configuration attributes for `outputFields` match the Address response object. For instance, street name can be populated using the [`thoroughfare`](https://api-typings.ideal-postcodes.co.uk/interfaces/address.html#thoroughfare) attribute. A list of address attributes provided by the API can be found at [@ideal-postcodes/api-typings](https://api-typings.ideal-postcodes.co.uk/interfaces/address.html).

More complex, dynamic assignment can be performed using the [`onAddressRetrieved`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onAddressRetrieved) callback.

Output fields assigned with a query selector are evaluated lazily (i.e. when an address attribute needs to be piped to a field).

[More information on addressing data can be found on our address data documentation](https://docs.ideal-postcodes.co.uk/docs/data).
