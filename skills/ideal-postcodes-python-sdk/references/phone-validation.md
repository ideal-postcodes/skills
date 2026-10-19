# Phone validation

`phone_number_validation` checks whether a phone number is valid and returns it in national and international format, with its country and network.

```python title="phone_validation.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    phone = client.phone_number_validation(query="+442071128019")
    if phone.valid:
        print(phone.international_format, phone.country, phone.original_carrier)
```

Include the country code. A UK number can start `+44`, `44` or `0044`. Your key needs phone validation enabled.

## Result

A valid number returns `valid=True` with:

| Field | Example |
| --- | --- |
| `national_format` | `020 7112 8019` |
| `international_format` | `+44 20 7112 8019` |
| `iso_country` / `iso_country_2` | `GBR` / `GB` |
| `country` | `United Kingdom` |
| `original_carrier` | The network the number was first assigned to, with `network_type` such as `mobile` or `landline` |

An invalid number returns `valid=False` and every other field `None`. It is not an error.

## Current network

Pass `current_carrier="true"` to fill `current_carrier` as well. A number can move to another network, so `original_carrier` alone can be out of date. The extra check can be slow, depending on the network, so allow for it in your `timeout`.

## Cost

One lookup per request, including numbers that are invalid or fail to parse.

See [`phone_number_validation`](https://docs.ideal-postcodes.co.uk/docs/sdks/python/endpoints/phone-number-validation) for the parameters.
