# `addresses`

Extract Addresses

Returns a list of UK addresses matching the query, ordered by relevance score. `limit` defaults to 10 and caps at 100. `page` defaults to 0.

You may only extract a single address from each request. To extract multiple addresses, you must perform an individual request for each address.

If the query is a valid postcode, the API returns the whole address list for that postcode and raises the limit to 100. Use `page` to reach the rest of a postcode holding more than 100 premises. You cannot page beyond 10,000 results.

Your key needs at least one of PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium. Without one the request is rejected.

A request that returns at least one address costs a lookup. Empty result sets are free.

## Reverse Geocoding

Return the addresses around a point with the `lon=` and `lat=` querystring arguments. The search radius is 100m and the API sorts addresses by distance from the point.

## Filters

Narrow your results by adding filters to your query string that correspond with an address attribute.

For instance, you can restrict to postcode `SW1A 2AA` by appending `postcode=sw1a2aa`.

If a filter term is invalid, e.g. `postcode=SW1A2AAA`, the API returns an empty result set and charges no lookup.

You can also scope using multiple terms for the same filter with a comma separated list of terms. E.g. Restrict results to E1, E2 and E3 outward codes: `postcode_outward=e1,e2,e3`. Multiple terms are `OR`'ed, i.e. the matching result sets are combined.

All filters can accept multiple terms unless stated otherwise below.

Multiple filters can also be combined. E.g. Restrict results to small user organisations in the N postcode area: `su_organisation_indicator=Y&postcode_area=n`. Multiple filters are `AND`'ed, i.e. each additional filter narrows the result set.

A combined maximum of 8 terms is allowed across all filters.

## Biases

You can boost address results that correspond with a given address attribute. All bias searches are prefixed with `bias_`.

Biased searches, unlike filtered searches, still allow unmatched addresses to appear. They rank lower.

For instance, you can boost addresses with postcode areas `SW` and `SE` by appending `bias_postcode_area=SW,SE`.

If a bias term is invalid, e.g. `bias_postcode=SW1A2AAA`, no bias is applied.

You may scope using multiple terms for the same bias with a comma separated list of terms. E.g. Prefer results in the `E1`, `E2` and `E3` outward codes: `bias_postcode_outward=e1,e2,e3`.

All biases can accept multiple terms unless stated otherwise below.

A combined maximum of 5 terms is allowed across all biases.

## Search by Postcode and Building Name or Number

Search by postcode and building attribute with the postcode filter and query argument. E.g. For "SW1A 2AA Prime Minister" `/v1/addresses?postcode=sw1a2aa&q=prime minister`.

Using a filter means a postcode mismatch returns no results and costs no lookup.

### Search by UPRN

Search by UPRN using the `uprn` filter and excluding the query argument. E.g. `/v1/addresses?uprn=100`.

## Endpoint

`GET /addresses`

## Import

```python
from ideal_postcodes import IdealPostcodes
```

Call `addresses` on the `IdealPostcodes` client. Path parameters are positional; query and body parameters are keyword-only.

## Path Parameters

_None._

## Query Parameters

| Name | Type | Required | Description |
| ---- | ---- | -------- | ----------- |
| `query` | `str` | no | Specifies the address to query. |
| `limit` | `int` | no | **Limit** Specifies the maximum number of records to retrieve. By default the limit is 10. Requesting a larger result set adds latency. |
| `page` | `int` | no | **Page** 0 indexed indicator of the page of results to receive. Virtually all postcode results are returned on page 0. A small number of Multiple Residence postcodes may need pagination (i.e. have more than 100 premises). |
| `filter` | `str` | no | **Restrict Result Fields** Comma separated whitelist of address elements to return. E.g. `filter=line_1,line_2,line_3` returns only the `line_1`, `line_2` and `line_3` address elements in your response. |
| `lon` | `float` | no | **Longitude** Longitude query for reverse geocoding. A valid reverse geocode query also needs a latitude (lat=) query. |
| `lat` | `float` | no | **Latitude** Latitude query for reverse geocoding. A valid reverse geocode query also needs a longitude (lon=) query. |
| `postcode_outward` | `str` | no | **Filter by Outward Code** Restrict result set to addresses with a matching outward code. The outward code is the first half of a postcode. E.g. the outward code for `SW1A 2AA` is `SW1A`. |
| `postcode` | `str` | no | **Filter by postcode** Restrict result set to matching postcodes only. Can be combined with query to perform a postcode and building number or name search. |
| `postcode_area` | `str` | no | **Filter by Postcode Area** Postcode area represents the first one or two non-numeric characters of a postcode. E.g. the postcode area of `SW1A 2AA` is `SW`. Can be combined with query to perform a postcode and building search. |
| `postcode_sector` | `str` | no | **Filter by Postcode Sector** Postcode sector is the outward code plus first numeric of the inward code. E.g. postcode sector of `SW1A 2AA` is `SW1A 2` |
| `post_town` | `str` | no | **Filter by Town or City** Restrict addresses to matching town, city or other locality identifier. |
| `uprn` | `int` | no | **Filter by UPRN** Does not accept comma separated terms. Only a single term is permitted. |
| `country` | `str` | no | **Filter by country** Filters by country name. In the GBR context, the country is never United Kingdom. It is England, Scotland, Wales, Northern Ireland, Jersey, Guernsey or Isle of Man. |
| `postcode_type` | `str` | no | **Filter by Postcode Type** Useful for separating organisational and residential addresses. |
| `su_organisation_indicator` | `str` | no | **Filter by Organisation Indicator** Useful for separating organisational and residential addresses. |
| `box` | `str` | no | **Filter by Bounding Box** Restrict search to a geospatial box determined by the "top-left" and "bottom-right" geolocations. Supply 4 comma separated values ordered `top_left_lon,top_left_lat,bottom_right_lon,bottom_right_lat`. The top-left longitude must be less than the bottom-right longitude, and the top-left latitude greater than the bottom-right latitude. A box which fails either check is ignored. Only one geospatial box can be provided. |
| `bias_postcode_outward` | `str` | no | **Bias by Outward Code** Boosts addresses with a matching outward code. The outward code is the first half of a postcode. For instance, the outward code of `SW1A 2AA` is `SW1A`. |
| `bias_postcode` | `str` | no | **Bias by postcode** Boost addresses which match postcode. Can be combined with query to perform a postcode and building number or name search. |
| `bias_postcode_area` | `str` | no | **Bias by Postcode Area** Boosts if the first one or two non-numeric characters of a postcode match The postcode areas of SW1A 2AA and N1 6RT are SW and N respectively. |
| `bias_postcode_sector` | `str` | no | **Bias by Postcode Sector** Boost postcode sector matches. The postcode sector comprises the outward code plus first numeric of the inward code. |
| `bias_post_town` | `str` | no | **Bias by Town or City** Biases results to matching town, city or other locality name. |
| `bias_thoroughfare` | `str` | no | **Bias by Street** Bias by street or thoroughfare name. |
| `bias_country` | `str` | no | **Bias by Country** Possible values are England, Scotland, Wales, Northern Ireland, Jersey, Guernsey and Isle of Man. |
| `bias_lonlat` | `str` | no | **Bias by Geolocation** Bias search to a geospatial circle determined by an origin and radius in metres. Max radius is `50000`. Uses the format bias_lonlat=[longitude],[latitude],[radius in metres]. Only one geospatial bias may be provided. |
| `tags` | `str` | no | **Tags** A comma separated list of tags to query over. Useful if you want to specify the circumstances in which the request was made. If you specify multiple tags, the response comprises only requests that satisfy all of them. Searching `"foo,bar"` queries only requests tagged both `"foo"` and `"bar"`. |
| `dataset` | `list[types.Dataset]` | no | **Filter by Dataset** Comma-separated list of datasets to search within. Filters results to only include addresses from the specified datasets. Useful for keys with multiple overlapping datasets enabled (e.g. `paf` and `abp`). |

## Response Type

```python
from ideal_postcodes.types import AddressesResult
```

With `filter`, returns `ProjectedAddressesResult`, whose `hits` are dictionaries of the requested fields.

## Errors

Unsuccessful HTTP responses raise `APIError` or a subclass, with the HTTP `status` and the API error `code` and `message`. Connection failures and timeouts raise `TransportError`. Unexpected responses raise `ProtocolError`. All derive from `ClientError`. See [error handling](https://docs.ideal-postcodes.co.uk/docs/sdks/python/errors).

## Example

```python title="addresses.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.addresses(query="BR8 7RE", page=0)
```
