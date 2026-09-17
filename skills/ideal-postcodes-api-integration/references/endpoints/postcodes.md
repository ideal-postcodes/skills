# Lookup Postcode

**Endpoint:** `GET /postcodes/{postcode}`

**Operation ID:** `Postcodes`

**Tags:** UK

Returns the complete list of addresses for a postcode. Postcode searches are space and case insensitive.

Each request looks up one postcode. To extract the addresses for several postcodes, send one request per postcode.

Use it to power postcode driven address searches, like [Postcode Lookup](/docs/postcode-lookup/).

Postcode lookup covers the United Kingdom, the Republic of Ireland, the Netherlands and Singapore. The API detects the format of the postcode you submit. UK and Irish postcodes are searched by default. For a Dutch or Singapore postcode, set `context` to `NLD` or `SGP`, or to `GLOBAL` to accept any supported format.

UK postcodes need PAF, Multiple Residence, Not Yet Built, PAF Alias, PAF Welsh, AddressBase or AddressBase Premium on your key. Eircodes need ECAD or ECAF. Dutch postcodes need Kadaster. Singapore postcodes need HERE Asia Pacific. Without a matching licence the request is rejected.

An unfound postcode costs no lookup. A postcode that returns addresses costs one.

## Postcode Not Found

Invalid postcodes do not affect your lookup balance. The API returns a `404` response with this body:

```json
{
  "code": 4040,
  "message": "Postcode not found",
  "suggestions": ["SW1A 0AA"]
}
```

### Suggestions

If a postcode cannot be found, the API returns up to 5 of the closest matching postcodes. It corrects common errors first (e.g. mixing up `O` and `0` or `I` and `1`).

If the suggestion list is small (fewer than 3), the correct postcode is likely to be among them. Notify the user or trigger new searches immediately.

The suggestion list is empty if the postcode has deviated too far from a valid postcode format.

## Multiple Residence

A small number of postcodes return more than 100 premises. The API returns 100 addresses per page, so use `page` to paginate the result set.

## Testing

- **ID1 1QD** Returns a successful postcode lookup response `2000`
- **ID1 KFA** Returns "postcode not found" error `4040`
- **ID1 CLIP** Returns "no lookups remaining" error `4020`
- **ID1 CHOP** Returns "daily (or individual) lookup limit breached" error `4021`

Test requests undergo the usual authentication and restriction rules. They surface any issues during implementation and do not cost you a lookup.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `postcode` | path | yes | string | Postcode to retrieve |
| `filter` | query | no | string | Comma separated whitelist of address elements to return. |
| `page` | query | no | integer | 0 indexed indicator of the page of results to receive. Virtually all postcode results are returned on page 0. |
| `tags` | query | no | string | A comma separated list of tags to query over. |
| `dataset` | query | no | array | Comma-separated list of datasets to search within. |
| `context` | query | no | string | Limits search results, typically within a country. |

## Request Samples

**curl**

```bash
curl -G 'https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A2AA' \
  -d 'api_key=ak_test'
```

**JavaScript**

```javascript
const response = await fetch(
  'https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A2AA?' +
  new URLSearchParams({
    api_key: 'ak_test',
  })
);

const { result } = await response.json();
```

**Python**

```python
import requests

response = requests.get(
    "https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A2AA",
    params={
        "api_key": "ak_test",
    },
)
result = response.json()["result"]
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A2AA")
uri.query = URI.encode_www_form(api_key: "ak_test")
result = JSON.parse(Net::HTTP.get(uri))["result"]
```

**PHP**

```php
<?php
$response = file_get_contents(
  "https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A2AA?" .
  http_build_query([
    "api_key" => "ak_test",
  ])
);
$result = json_decode($response, true)["result"];
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `result` | yes | array<[AddressListItem](../data/address-list-item.md)> | All addresses listed at the postcode. |
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |
| `page` | yes | integer |  |
| `limit` | yes | integer |  |
| `total` | yes | integer |  |

## Response Example

```json
{
  "result": [
    {
      "postcode": "SW1A 2AA",
      "postcode_inward": "2AA",
      "postcode_outward": "SW1A",
      "post_town": "London",
      "dependant_locality": "",
      "double_dependant_locality": "",
      "thoroughfare": "Downing Street",
      "dependant_thoroughfare": "",
      "building_number": "10",
      "building_name": "",
      "sub_building_name": "",
      "po_box": "",
      "department_name": "",
      "organisation_name": "Prime Minister & First Lord Of The Treasury",
      "udprn": 23747771,
      "postcode_type": "S",
      "su_organisation_indicator": "",
      "delivery_point_suffix": "1A",
      "line_1": "Prime Minister & First Lord Of The Treasury",
      "line_2": "10 Downing Street",
      "line_3": "",
      "premise": "10",
      "longitude": -0.12767,
      "latitude": 51.503541,
      "eastings": 530047,
      "northings": 179951,
      "country": "England",
      "traditional_county": "Greater London",
      "administrative_county": "",
      "postal_county": "London",
      "county": "London",
      "district": "Westminster",
      "ward": "St. James's",
      "uprn": "100023336956",
      "id": "paf_23747771",
      "country_iso": "GBR",
      "country_iso_2": "GB",
      "county_code": "",
      "language": "en",
      "umprn": "",
      "dataset": "paf"
    }
  ],
  "code": 2000,
  "message": "Success",
  "limit": 100,
  "page": 0,
  "total": 1
}
```

## Error Status Codes

| HTTP | Code | Message |
|---|---|---|
| 404 | 4040 | Postcode not found |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/postcodes)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
