# `find_address`

Find Address

Returns address suggestions for a partial address, ordered by relevance. Use it to power real-time address autofill.

Consider our address autocomplete JavaScript libraries, which add address lookup to a form without calling this API directly.

## API Usage

Implementing our Address Autocomplete API involves:

1. Fetch address suggestions with `/autocomplete/addresses`
2. Acquire the complete address using the ID from the suggestion

Step 2 decrements your lookup balance.

Step 1 is not a free standalone resource. We rate limit and then suspend integrations that repeatedly make autocomplete requests without a paid Step 2 request.

## Context

`context` limits the search, usually to a single country. It defaults to `GBR`, and an unrecognised context falls back to that default. If your key is not licensed for the datasets covering the context, the request is rejected.

Querying a full postcode within a supported context returns the entire address list for that postcode.

## Query Filters

Refine results by appending filters to your querystring, e.g. `postcode=sw1a2aa` for postcode `SW1A 2AA`. Invalid filters return an empty set without affecting your lookup count.

To apply multiple filter terms, use a comma-separated list, e.g. `postcode_outward=e1,e2,e3` combines result sets for E1, E2 and E3. Unless otherwise specified, all filters support multiple terms.

Filters combine with `AND` logic, for instance `su_organisation_indicator=Y&postcode_area=n`. The maximum is **8** filter terms.

## Address Bias

Preface bias searches with `bias_` to boost certain address results. Unlike filters, biasing allows unmatched addresses to appear with lower priority.

For example, use `bias_postcode_area=SW,SE` to favour addresses in the `SW` and `SE` postcode areas. Invalid bias terms have no effect.

Multiple bias terms are allowed unless stated otherwise, with a combined maximum of **5**.

## Suggestion Format

The suggestion format is subject to change. We recommend using the suggestion as-is to avoid integration issues.

## Rate Limiting and Cost

The default rate limit is 3,000 requests per 5 minutes, counted per key and IP address. The `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` headers report where you stand.

Autocomplete API usage does not impact your balance, but resolving a suggestion to a full address requires a paid request. Autocomplete requests without subsequent paid requests may lead to rate limiting or suspension.

## Endpoint

`GET /autocomplete/addresses`

See the [API reference](https://docs.ideal-postcodes.co.uk/docs/api/find-address) for this endpoint.

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `find_address` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

_None._

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `query` | `str` | yes | **Address Query** The partial address string entered by the user to autocomplete. |
| `dataset` | `list[types.Dataset]` | no | **Filter by Dataset** Comma-separated list of datasets to search within. Filters results to only include addresses from the specified datasets. Useful for keys with multiple overlapping datasets enabled (e.g. `paf` and `abp`). |
| `context` | `str` | no | **Context** Limits search results, typically within a country. |
| `limit` | `int` | no | **Limit** Specifies the maximum number of records to retrieve. By default the limit is 10. Requesting a larger result set adds latency. |
| `bias_lonlat` | `str` | no | **Bias by Geolocation** Bias search to a geospatial circle determined by an origin and radius in metres. Max radius is `50000`. Uses the format bias_lonlat=[longitude],[latitude],[radius in metres]. Only one geospatial bias may be provided. |
| `bias_ip` | `Literal['true'] \| str` | no | **Bias by Geolocation of IP** Biases search based on approximate geolocation of IP address. Set `bias_ip=true` to enable. |
| `box` | `str` | no | **Filter by Bounding Box** Restrict search to a geospatial box determined by the "top-left" and "bottom-right" geolocations. Supply 4 comma separated values ordered `top_left_lon,top_left_lat,bottom_right_lon,bottom_right_lat`. The top-left longitude must be less than the bottom-right longitude, and the top-left latitude greater than the bottom-right latitude. A box which fails either check is ignored. Only one geospatial box can be provided. |
| `postcode_outward` | `str` | no | **Filter by Outward Code** Restrict result set to addresses with a matching outward code. The outward code is the first half of a postcode. E.g. the outward code for `SW1A 2AA` is `SW1A`. |
| `postcode` | `str` | no | **Filter by postcode** Restrict result set to matching postcodes only. Can be combined with query to perform a postcode and building number or name search. |
| `postcode_area` | `str` | no | **Filter by Postcode Area** Postcode area represents the first one or two non-numeric characters of a postcode. E.g. the postcode area of `SW1A 2AA` is `SW`. Can be combined with query to perform a postcode and building search. |
| `postcode_sector` | `str` | no | **Filter by Postcode Sector** Postcode sector is the outward code plus first numeric of the inward code. E.g. postcode sector of `SW1A 2AA` is `SW1A 2` |
| `post_town` | `str` | no | **Filter by Town or City** Restrict addresses to matching town, city or other locality identifier. |
| `uprn` | `int` | no | **Filter by UPRN** Does not accept comma separated terms. Only a single term is permitted. |
| `country` | `str` | no | **Filter by country** Filters by country name. In the GBR context, the country is never United Kingdom. It is England, Scotland, Wales, Northern Ireland, Jersey, Guernsey or Isle of Man. |
| `postcode_type` | `str` | no | **Filter by Postcode Type** Useful for separating organisational and residential addresses. |
| `su_organisation_indicator` | `str` | no | **Filter by Organisation Indicator** Useful for separating organisational and residential addresses. |
| `bias_postcode_outward` | `str` | no | **Bias by Outward Code** Boosts addresses with a matching outward code. The outward code is the first half of a postcode. For instance, the outward code of `SW1A 2AA` is `SW1A`. |
| `bias_postcode` | `str` | no | **Bias by postcode** Boost addresses which match postcode. Can be combined with query to perform a postcode and building number or name search. |
| `bias_postcode_area` | `str` | no | **Bias by Postcode Area** Boosts if the first one or two non-numeric characters of a postcode match The postcode areas of SW1A 2AA and N1 6RT are SW and N respectively. |
| `bias_postcode_sector` | `str` | no | **Bias by Postcode Sector** Boost postcode sector matches. The postcode sector comprises the outward code plus first numeric of the inward code. |
| `bias_post_town` | `str` | no | **Bias by Town or City** Biases results to matching town, city or other locality name. |
| `bias_thoroughfare` | `str` | no | **Bias by Street** Bias by street or thoroughfare name. |
| `bias_country` | `str` | no | **Bias by Country** Possible values are England, Scotland, Wales, Northern Ireland, Jersey, Guernsey and Isle of Man. |
| `postal_code` | `str` | no | **Filter by postal code** Restrict results to addresses with a matching full postal code. Case, spaces and hyphens are ignored. For US addresses the full postal code is the nine digit ZIP+4 (`941021234`); filter on `postal_code_3` for a five digit ZIP. For UK addresses use `postcode`. |
| `postal_code_2` | `str` | no | **Filter by postal code prefix** Restrict results to addresses whose postal code starts with the given segment. For US addresses this is the three digit ZIP prefix (sectional center), e.g. `941` for San Francisco. |
| `postal_code_3` | `str` | no | **Filter by short postal code** Restrict results to addresses with a matching short postal code. For US addresses this is the five digit ZIP code. |
| `city` | `str` | no | **Filter by city** Restrict results to addresses in the named city, town or locality. Case, spaces and accents are ignored, so `San Francisco` and `sanfrancisco` match the same addresses. For UK addresses use `post_town`. |
| `state` | `str` | no | **Filter by state** Restrict results to addresses in the named state, province or region, e.g. `California`. Case and spaces are ignored. |
| `state_code` | `str` | no | **Filter by state code** Restrict results to addresses with a matching state or region code, e.g. the two letter USPS state abbreviation `CA`. Case is ignored. |
| `bias_postal_code` | `str` | no | **Bias by postal code** Boost addresses with a matching full postal code (nine digit ZIP+4 for US addresses). Unmatched addresses still appear, ranked lower. |
| `bias_postal_code_2` | `str` | no | **Bias by postal code prefix** Boost addresses whose postal code starts with the given segment (three digit ZIP prefix for US addresses). |
| `bias_postal_code_3` | `str` | no | **Bias by short postal code** Boost addresses with a matching short postal code (five digit ZIP for US addresses). |
| `bias_city` | `str` | no | **Bias by city** Boost addresses in the named city, town or locality. Case, spaces and accents are ignored. For UK addresses use `bias_posttown`. |
| `bias_state` | `str` | no | **Bias by state** Boost addresses in the named state, province or region. |
| `bias_state_code` | `str` | no | **Bias by state code** Boost addresses with a matching state or region code, e.g. `CA`. |
| `is_pobox` | `Literal['true', 'false'] \| str` | no | **Filter by PO Box** `true` restricts results to PO Box addresses; `false` excludes them. For US addresses this is derived from the USPS record type (`P`). |
| `is_business` | `Literal['true', 'false'] \| str` | no | **Filter by business address** `true` restricts results to business addresses; `false` excludes them. For US addresses this is derived from the USPS record type (`F`, a firm record). |

## Response Type

```python
from ideal_postcodes.types import FindAddressResult
```

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="find_address.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    suggestions = client.find_address(query="9 Malyons Road")
```
