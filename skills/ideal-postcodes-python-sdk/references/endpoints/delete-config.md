# `delete_config`

Delete

Permanently deletes a configuration object.

## Endpoint

`DELETE /keys/{key}/configs/{config}`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/delete-config) for this endpoint.

This operation needs a Management Key. Pass it as the required `user_token` keyword argument; the client sends it in the `Authorization` header. See [setup](https://docs.ideal-postcodes.co.uk/docs/sdks/python/setup).

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `delete_config` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `key` | `str` | yes | **API Key** The API Key. Begins `ak_`. |
| `config` | `str` | yes | **Configuration Name** User-provided configuration object name. |

## Query Parameters

_None._

## Response Type

```python
from ideal_postcodes.types import DeleteConfigResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="delete_config.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.delete_config("ak_test", "checkout", user_token="uk_example")
```
