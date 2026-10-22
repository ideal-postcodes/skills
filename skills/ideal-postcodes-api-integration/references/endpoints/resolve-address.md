# Resolve Address

**Endpoint:** `GET /autocomplete/addresses/{address}/gbr`

**Operation ID:** `ResolveAddress`

**Tags:** Address Search

Returns the complete address for an autocomplete suggestion, identified by its address ID.

This is the step of the autocomplete flow that costs a lookup. Fetching suggestions is free.

The API returns resolved addresses, including addresses outside the UK, in a UK format (up to 3 address lines) using UK nomenclature such as postcode and county.

An ID that matches no address returns `404`.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `address` | path | yes | string | ID of address suggestion provided by the API to fully resolve. |
| `tags` | query | no | string | A comma separated list of tags to query over. |

## Request Samples

**curl**

```bash
curl -G 'https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses/paf_23747771/gbr' \
  -d 'api_key=ak_test'
```

**JavaScript**

```javascript
import { createIdpcClient } from '@ideal-postcodes/sdk';
import { createAutocomplete } from '@ideal-postcodes/sdk/autocomplete';

const client = createIdpcClient({ apiKey: 'ak_test' });

// Debounces keystrokes, drops superseded requests and caches suggestions
const autocomplete = createAutocomplete({ client });

const hits = await autocomplete.find({ query: '10 downing' });
if (hits?.length) {
  const address = await autocomplete.resolve({ hit: hits[0] });
}
```

**JavaScript (direct)**

```javascript
import { createIdpcClient, resolveAddress } from '@ideal-postcodes/sdk';

const client = createIdpcClient({ apiKey: 'ak_test' });

const { data } = await resolveAddress({
  client,
  path: { address: 'paf_23747771' },
});
```

**Python**

```python
# pip install ideal-postcodes
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.resolve_address("paf_23747771")
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses/paf_23747771/gbr")
uri.query = URI.encode_www_form(api_key: "ak_test")
result = JSON.parse(Net::HTTP.get(uri))["result"]
```

**PHP**

```php
<?php
$response = file_get_contents(
  "https://api.ideal-postcodes.co.uk/v1/autocomplete/addresses/paf_23747771/gbr?" .
  http_build_query([
    "api_key" => "ak_test",
  ])
);
$result = json_decode($response, true)["result"];
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |
| `result` | yes | [Address](../data/address.md) | The standard Ideal Postcodes address, which maps both UK and International addresses. |

## Response Example

```json
{
  "code": 2000,
  "message": "Success",
  "result": {
    "id": "paf_23747771",
    "dataset": "paf",
    "country_iso": "GBR",
    "country_iso_2": "GB",
    "country": "England",
    "language": "en",
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
    "umprn": "",
    "uprn": "100023336956",
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
    "county": "London",
    "county_code": "",
    "traditional_county": "Greater London",
    "administrative_county": "",
    "postal_county": "London",
    "district": "Westminster",
    "ward": "St. James's",
    "native": {
      "dataset": "paf",
      "postcode": "SW1A 2AA",
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
      "delivery_point_suffix": "1A"
    }
  }
}
```

## Error Status Codes

| HTTP | Code | Message |
|---|---|---|
| 404 |  | Resource not found |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/resolve-address)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
