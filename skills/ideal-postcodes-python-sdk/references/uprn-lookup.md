# UPRN lookup

To get the address for a UPRN, call `addresses` with the `uprn` filter and no `query`. The UPRN (Unique Property Reference Number) is the number UK local authorities assign to each property and Ordnance Survey maintains.

```python title="uprn_lookup.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    hits = client.addresses(uprn=50002012147).hits
    if hits:
        print(hits[0].line_1, hits[0].post_town, hits[0].postcode)
```

This prints `9 Malyons Road Swanley BR8 7RE`.

## Several addresses per UPRN

A key with PAF Alias returns the Royal Mail record plus each alias for the same property. UPRN `1`, Bristol City Council, returns three hits: the `paf` record and two `pafa` aliases with different department lines. Check `dataset` if you want the Royal Mail record only.

## UPRN not found

`hits` is empty and the request costs nothing. It does not raise an error.

## One UPRN per request

The `uprn` filter does not take a comma-separated list, so send one request per UPRN.

## Cost

One lookup when the UPRN matches an address. Your key needs one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium.

See [`addresses`](https://docs.ideal-postcodes.co.uk/docs/sdks/python/endpoints/addresses) for the other filters, such as `postcode_outward` and `post_town`.
