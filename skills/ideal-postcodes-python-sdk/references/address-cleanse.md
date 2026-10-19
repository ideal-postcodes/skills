# Address cleanse

`address_cleanse` matches a freeform UK address to the closest address on file and scores the match. Use it to clean addresses you already hold, such as a CRM export.

```python title="address_cleanse.py"
from ideal_postcodes import IdealPostcodes, types

with IdealPostcodes("ak_test") as client:
    result = client.address_cleanse(query="9 Malyons Rd, Swanley, BR8 7RE")
    if isinstance(result, types.GbrCleanseMatch) and result.confidence > 0:
        print(result.match.line_1, result.match.postcode, result.confidence)
    else:
        print("No match")
```

## Match or no match

Check the type and the score. A match is a `GbrCleanseMatch` with a `confidence` above 0. A no-match is a `GbrCleanseNoMatch`, with every score at 0 and every match level at `NO_MATCH`.

## Separate postcode and post town

If your records keep the postcode or post town in their own fields, pass them as `postcode` and `post_town` alongside `query` rather than joining them into it.

## Confidence threshold

It depends on your data. Each wrong, missing or misspelt element lowers the score, and one source tends to repeat the same mistakes. Cleanse a sample, sort it by `confidence` and set your threshold where the matches start to go wrong.

The result also scores each element separately, in `organisation_match`, `premise_match`, `thoroughfare_match`, `locality_match`, `post_town_match` and `postcode_match`. The [API reference](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse) explains each level.

## Cost

One lookup for a match. A no-match costs nothing.
