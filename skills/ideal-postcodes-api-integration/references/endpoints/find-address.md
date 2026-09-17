# Find Address

**Endpoint:** `GET /autocomplete/addresses`

**Operation ID:** `FindAddress`

**Tags:** Address Search

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

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `query` | query | no | string | The partial address string entered by the user to autocomplete. |
| `dataset` | query | no | array | Comma-separated list of datasets to search within. |
| `context` | query | no | string | Limits search results, typically within a country. |
| `limit` | query | no | integer | Specifies the maximum number of records to retrieve. |
| `bias_lonlat` | query | no | string | Bias search to a geospatial circle determined by an origin and radius in metres. Max radius is `50000`. |
| `bias_ip` | query | no | `true` | Biases search based on approximate geolocation of IP address. |
| `box` | query | no | string | Restrict search to a geospatial box determined by the "top-left" and "bottom-right" geolocations. |
| `postcode_outward` | query | no | string | Restrict result set to addresses with a matching outward code. |
| `postcode` | query | no | string | Restrict result set to matching postcodes only. |
| `postcode_area` | query | no | string | Postcode area represents the first one or two non-numeric characters of a postcode. E.g. the postcode area of `SW1A 2AA` is `SW`. |
| `postcode_sector` | query | no | string | Postcode sector is the outward code plus first numeric of the inward code. E.g. postcode sector of `SW1A 2AA` is `SW1A 2` |
| `post_town` | query | no | string | Restrict addresses to matching town, city or other locality identifier. |
| `uprn` | query | no | integer | Does not accept comma separated terms. Only a single term is permitted. |
| `country` | query | no | string | Filters by country name. |
| `postcode_type` | query | no | string | Useful for separating organisational and residential addresses. |
| `su_organisation_indicator` | query | no | string | Useful for separating organisational and residential addresses. |
| `bias_postcode_outward` | query | no | string | Boosts addresses with a matching outward code. |
| `bias_postcode` | query | no | string | Boost addresses which match postcode. |
| `bias_postcode_area` | query | no | string | Boosts if the first one or two non-numeric characters of a postcode match |
| `bias_postcode_sector` | query | no | string | Boost postcode sector matches. The postcode sector comprises the outward code plus first numeric of the inward code. |
| `bias_post_town` | query | no | string | Biases results to matching town, city or other locality name. |
| `bias_thoroughfare` | query | no | string | Bias by street or thoroughfare name. |
| `bias_country` | query | no | string | Possible values are England, Scotland, Wales, Northern Ireland, Jersey, Guernsey and Isle of Man. |
| `postal_code` | query | no | string | Restrict results to addresses with a matching full postal code. Case, spaces and hyphens are ignored. For US addresses the full postal code is the nine digit ZIP+4 (`941021234`); filter on `postal_code_3` for a five digit ZIP. For UK addresses use `postcode`. |
| `postal_code_2` | query | no | string | Restrict results to addresses whose postal code starts with the given segment. For US addresses this is the three digit ZIP prefix (sectional center), e.g. `941` for San Francisco. |
| `postal_code_3` | query | no | string | Restrict results to addresses with a matching short postal code. For US addresses this is the five digit ZIP code. |
| `city` | query | no | string | Restrict results to addresses in the named city, town or locality. Case, spaces and accents are ignored, so `San Francisco` and `sanfrancisco` match the same addresses. For UK addresses use `post_town`. |
| `state` | query | no | string | Restrict results to addresses in the named state, province or region, e.g. `California`. Case and spaces are ignored. |
| `state_code` | query | no | string | Restrict results to addresses with a matching state or region code, e.g. the two letter USPS state abbreviation `CA`. Case is ignored. |
| `bias_postal_code` | query | no | string | Boost addresses with a matching full postal code (nine digit ZIP+4 for US addresses). Unmatched addresses still appear, ranked lower. |
| `bias_postal_code_2` | query | no | string | Boost addresses whose postal code starts with the given segment (three digit ZIP prefix for US addresses). |
| `bias_postal_code_3` | query | no | string | Boost addresses with a matching short postal code (five digit ZIP for US addresses). |
| `bias_city` | query | no | string | Boost addresses in the named city, town or locality. Case, spaces and accents are ignored. For UK addresses use `bias_posttown`. |
| `bias_state` | query | no | string | Boost addresses in the named state, province or region. |
| `bias_state_code` | query | no | string | Boost addresses with a matching state or region code, e.g. `CA`. |
| `is_pobox` | query | no | `true` \| `false` | `true` restricts results to PO Box addresses; `false` excludes them. For US addresses this is derived from the USPS record type (`P`). |
| `is_business` | query | no | `true` \| `false` | `true` restricts results to business addresses; `false` excludes them. For US addresses this is derived from the USPS record type (`F`, a firm record). |

## Request Samples

**curl**

```bash
curl -G 'https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses' \
  -d 'api_key=ak_test' \
  --data-urlencode 'query=10 downing'
```

**JavaScript**

```javascript
const response = await fetch(
  'https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses?' +
  new URLSearchParams({
    api_key: 'ak_test',
    query: '10 downing',
  })
);

const { result } = await response.json();
```

**Python**

```python
import requests

response = requests.get(
    "https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses",
    params={
        "api_key": "ak_test",
        "query": "10 downing",
    },
)
result = response.json()["result"]
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses")
uri.query = URI.encode_www_form(api_key: "ak_test", query: "10 downing")
result = JSON.parse(Net::HTTP.get(uri))["result"]
```

**PHP**

```php
<?php
$response = file_get_contents(
  "https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses?" .
  http_build_query([
    "api_key" => "ak_test",
    "query" => "10 downing",
  ])
);
$result = json_decode($response, true)["result"];
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `result` | yes | object |  |
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |

## Response Example

```json
{
  "result": {
    "hits": [
      {
        "id": "paf_23747771",
        "suggestion": "Prime Minister & First Lord Of The Treasury, 10 Downing Street, London, SW1A",
        "udprn": 23747771,
        "urls": {
          "udprn": "/v1/udprn/23747771"
        }
      },
      "… 1 more items …"
    ]
  },
  "code": 2000,
  "message": "Success"
}
```

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/find-address)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
