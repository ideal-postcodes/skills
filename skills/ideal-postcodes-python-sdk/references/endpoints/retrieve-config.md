# `retrieve_config`

Retrieve

Returns a configuration by name. This request needs no `user_token`, so a browser integration can read its own configuration at runtime.

## Endpoint

`GET /keys/{key}/configs/{config}`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/retrieve-config) for this endpoint.

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `retrieve_config` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `str` | yes | **API Key** The API Key. Begins `ak_`. |
| `config` | `str` | yes | **Configuration Name** User-provided configuration object name. |

## Query Parameters

_None._

## Response Type

```python
from ideal_postcodes.types import RetrieveConfigResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="retrieve_config.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    config = client.retrieve_config("ak_test", "checkout")
```
