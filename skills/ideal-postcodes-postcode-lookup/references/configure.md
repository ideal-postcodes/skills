# Configure

Postcode Lookup exports a `setup` method to apply address verification to a form. `setup` requires the following configuration at minimum.

> `apiKey`, `context` and `outputFields` below are all you need to get Postcode Lookup working. For the complete set of controller options, see [Configuration Reference](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/configuration-reference).

## API Key

`apiKey`

API Key from your Ideal Postcodes account. Typically begins `ak_`

## Postcode Lookup Container

`context`

Specify an area on your page where Postcode Lookup can render its user interface. Typically a `<div>`, you may reference this using a CSS Selector or a direct reference to the node.

```javascript
{
  context: "#context_id"
}
```

## Address Targets

`outputFields`

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

Assigning up to 3 address lines, post town and postcode fields, is all addressing information required to identify a UK premise. You may extract more data for an address by passing more properties into the `outputFields` configuration object.

The configuration attributes for `outputFields` match the Address response object. For instance, street name can be populated using the [`thoroughfare`](https://docs.ideal-postcodes.co.uk/docs/data/paf#thoroughfare) attribute. A list of address attributes provided by the API can be found in our [UK address data guide](https://docs.ideal-postcodes.co.uk/docs/data/paf).

More complex, dynamic assignment can be performed using the `onAddressSelected` callback.

Output fields assigned with a query selector are evaluated lazily (i.e. when an address attribute needs to be piped to a field).

[More information on addressing data can be found on our address data documentation](https://docs.ideal-postcodes.co.uk/docs/data).
