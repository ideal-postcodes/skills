# Responses, arguments and testing

Every method takes the API's parameters as Python arguments and returns the API's `result` as a Pydantic model.

## Return values

The `result` field of the API response, as a Pydantic v2 model or a list of models. Read fields as attributes, or call `model_dump()` for a dictionary. If the API adds a field your SDK version does not know, it still appears in `model_dump()`. Option fields are plain strings, so new values come through. A `native` record from a dataset your SDK version does not know arrives as a dictionary.

`key_logs` returns CSV text as a `str` instead. With `filter`, `postcodes` returns a list of dictionaries and `addresses` returns a result whose `hits` are dictionaries, each holding only the fields you asked for.

## Arguments

Path parameters, such as the postcode or the key, are positional. Everything else is keyword-only. The SDK leaves out any argument set to `None`, so the API applies its default, and sends `False`, `0` and `""` as given. Lists, such as `dataset`, are joined with commas.

The SDK checks ranges, lengths and allowed values from the spec before it sends anything, and raises `ValueError` if one fails. `help(IdealPostcodes.addresses)` lists every argument and its limits.

## Types

Response models are in `ideal_postcodes.types`, such as `Address` and `AddressListItem`. Typed dictionaries for nested settings are in `ideal_postcodes.requests`. The package ships `py.typed`, so type checkers read them without stubs.

## Testing without the network

Pass an `httpx.MockTransport` as `transport`. Every request goes to your handler instead of the API.

```python title="test_lookup.py"
import httpx
import pytest

from ideal_postcodes import IdealPostcodes, NotFoundError

def not_found(request: httpx.Request) -> httpx.Response:
    return httpx.Response(404, json={"code": 4040, "message": "Postcode not found", "suggestions": ["BR8 7RE"]})

def test_unknown_postcode() -> None:
    with IdealPostcodes("ak_test", transport=httpx.MockTransport(not_found)) as client:
        with pytest.raises(NotFoundError) as error:
            client.postcodes("BR8 7RX")
    assert error.value.suggestions == ["BR8 7RE"]
```

A mocked success needs the API's envelope, `{"code": 2000, "result": ...}`, around a result that matches the response schema. Anything else raises `ProtocolError`.
