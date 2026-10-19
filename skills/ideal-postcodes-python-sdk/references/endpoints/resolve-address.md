# `resolve_address`

Resolve Address

Returns the complete address for an autocomplete suggestion, identified by its address ID.

This is the step of the autocomplete flow that costs a lookup. Fetching suggestions is free.

The API returns resolved addresses, including addresses outside the UK, in a UK format (up to 3 address lines) using UK nomenclature such as postcode and county.

An ID that matches no address returns `404`.

## Endpoint

`GET /autocomplete/addresses/{address}/gbr`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/resolve-address) for this endpoint.

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `resolve_address` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `address` | `str` | yes | **ID of address suggestion** ID of address suggestion provided by the API to fully resolve. |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `tags` | `str` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |

## Response Type

```python
from ideal_postcodes.types import ResolveAddressResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="resolve_address.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    address = client.resolve_address("paf_2670849")
```
