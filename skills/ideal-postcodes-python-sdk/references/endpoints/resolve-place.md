# `resolve_place`

Resolve Place

Returns the full place for a place ID taken from a `/places` suggestion.

On top of the fields carried by the suggestion, the response adds coordinates, language and the underlying dataset record.

Each request decrements your lookup balance. An unknown ID returns `404`.

## Endpoint

`GET /places/{place}`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/resolve-place) for this endpoint.

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `resolve_place` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `place` | `str` | yes | ID of place suggestion |

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `tags` | `str` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |

## Response Type

```python
from ideal_postcodes.types import ResolvePlaceResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="resolve_place.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    place = client.resolve_place("geonames_2643743")
```
