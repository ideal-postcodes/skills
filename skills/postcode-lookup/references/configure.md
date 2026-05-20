# Configure

Postcode Lookup exports a [`setup`](https://postcode-lookup.ideal-postcodes.co.uk/modules/Setup.html#setup) method to apply address verification to a form. [`setup`](https://postcode-lookup.ideal-postcodes.co.uk/modules/Setup.html#setup) requires the following configuration at minimum.

## API Key

[`apiKey`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#apiKey)

API Key from your Ideal Postcodes account. Typically begins `ak_`

## Postcode Lookup Container

[`context`](https://postcode-lookup.ideal-postcodes.co.uk/classes/Controller.Controller-1.html#context)

Specify an area on your page where Postcode Lookup can render the necessary user interface to allow Postcode Lookup. Typically a `<div>`, you may reference this using a CSS Selector or a direct reference to the node.

```javascript
{
  context: "#context_id"
}
```

## Address Targets

[`outputFields`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#outputFields)

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

Assigning up to 3 address lines, post town and postcode fields, is all addressing information required to identify a UK premise. You may extract more data for an address by passing more properties into the `outputFields` configuration object.

The configuration attributes for `outputFields` matches the Address response object. For instance, street name can be populated using the [`thoroughfare`](https://api-typings.ideal-postcodes.co.uk/interfaces/address.html#thoroughfare) attribute. A list of address attributes provided by the API can be found at [@ideal-postcodes/api-typings](https://api-typings.ideal-postcodes.co.uk/interfaces/address.html).

More complex, dynamic assignment can be performed using the [`onAddressSelected`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#onAddressSelected) callback.

Output fields assigned with a query selector are evaluated lazily (i.e. when an address attribute needs to be piped to a field).

[More information on addressing data can be found on our address data documentation](https://docs.ideal-postcodes.co.uk/docs/data).
