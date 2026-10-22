# `key_details`

Details

Returns private data on a key: remaining lookups, licensed datasets, usage limits, notification settings and the search contexts the key can serve.

## Endpoint

`GET /keys/{key}/details`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/key-details) for this endpoint.

This operation needs a Management Key. Pass it as the required `user_token` keyword argument; the client sends it in the `Authorization` header. See [setup](https://docs.ideal-postcodes.co.uk/docs/sdks/python/setup).

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `key_details` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `str` | yes | **API Key** The API Key. Begins `ak_`. |

## Query Parameters

_None._

## Response Type

```python
from ideal_postcodes.types import KeyDetailsResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="key_details.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    details = client.key_details("ak_test", user_token="uk_example")
```
