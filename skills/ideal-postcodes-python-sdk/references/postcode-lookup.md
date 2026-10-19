# Postcode lookup

`postcodes` returns every address at a UK postcode. It ignores case and spacing, so `br87re` finds `BR8 7RE`.

```python title="postcode_lookup.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    for address in client.postcodes("BR8 7RE"):
        print(address.line_1, address.post_town, address.postcode)
```

## Result

A list of `AddressListItem` models, one per address. Each carries the formatted lines `line_1` to `line_3`, `post_town`, `postcode`, `uprn`, `udprn`, `longitude` and `latitude`. 9 Malyons Road returns `uprn="50002012147"` and `udprn=2670849`.

Pass `filter` to return only the fields you need, such as `filter="line_1,post_town,postcode"`. The method then returns a list of dictionaries instead of models.

## Unknown postcodes

The call raises `NotFoundError`, API code `4040`, and costs nothing. `error.suggestions` holds up to 5 of the closest real postcodes. With fewer than 3 suggestions, the right one is usually among them.

```python title="not_found.py"
from ideal_postcodes import IdealPostcodes, NotFoundError

with IdealPostcodes("ak_test") as client:
    try:
        addresses = client.postcodes("BR8 7RX")
    except NotFoundError as error:
        print("Did you mean", error.suggestions)
```

## Cost

One lookup for a postcode that returns addresses. Your key needs one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium.

## Pagination

The API returns 100 addresses per page. Pass `page=1` for the next 100. Only a few large Multiple Residence postcodes need it.

Irish, Dutch and Singapore postcodes need `context`. See [`postcodes`](https://docs.ideal-postcodes.co.uk/docs/sdks/python/endpoints/postcodes).
