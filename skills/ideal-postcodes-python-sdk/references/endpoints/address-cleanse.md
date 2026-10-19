# `address_cleanse`

Cleanse Address

Returns the closest matching address for a freeform address input, with Match Level indicators describing how closely each element of the suggested address matches the input. The more impaired the input address, the harder it is to cleanse.

A cleanse that returns a match costs a lookup. A no-match response is free.

## Countries

Cleanse defaults to the UK. Pass `context` with an ISO 3166-1 alpha-3 country
code to cleanse an address elsewhere, e.g. `context=USA` or `context=FRA`. The
address datasets your key is licensed for decide which countries it can
cleanse.

The response shape is the same for every country: a standardised address in
`match`, the raw source record in `match.native`, and the same confidence,
fit and Match Level indicators. The Match Levels and confidence score are at
their most discriminating for the UK, where cleanse runs against
purpose-built indexes; elsewhere they are computed from the country's address
dataset.

## Confidence Score

Each incorrect, missing or misspelled element subtracts from the overall confidence score.

### Deciding on an Acceptable Confidence Score Threshold

Inputs differ widely between address cleanse projects. Within a project, though, they tend to repeat the same errors. Some datasets are keyed in by hand and prone to typos. Others have a persistently missing datapoint such as organisation name or postcode. There is no absolute Confidence Score threshold. Set the acceptable score project by project, based on the systematic errors in the data and your business goals.

To set a threshold, load a subset of the dataset into a spreadsheet application like Excel and sort on the score. Scrolling from top to bottom shows matches from best to worst. As you reach the lower quality searches you can judge roughly:

- Which confidence scores indicate ambiguous matches (i.e. up to building level only)
- Which confidence scores indicate a poor or no match (i.e. the nearest matching address is too far from the input address)

Depending on your business goals, you can also use the Match Levels to determine an acceptable match. You may need to match only up to the thoroughfare or building name, or accurate organisation names may matter.

## Endpoint

`POST /cleanse/addresses`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse) for this endpoint.

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `address_cleanse` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

_None._

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `tags` | `str` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |
| `context` | `str` | no | Identify the country of the address to cleanse. Defaults to UK (GBR) |

## Request Body

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `query` | `str` | yes | Freeform address input to cleanse |
| `postcode` | `str` | no | Optionally specify the postal code for the address. |
| `post_town` | `str` | no | Optionally specify the city or town of the address. This should be the "post town" of the address. |
| `county` | `str` | no | Optionally specify the county of the address. We recommend omitting this field as county data is unreliable. |

## Response Type

```python
from ideal_postcodes.types import AddressCleanseResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="address_cleanse.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.address_cleanse(query="9 Malyons Road, Swanley, BR8 7RE")
```
