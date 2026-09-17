# Cleanse Address

**Endpoint:** `POST /cleanse/addresses`

**Operation ID:** `AddressCleanse`

**Tags:** UK

Returns the closest matching address for a freeform address input, with Match Level indicators describing how closely each element of the suggested address matches the input. The more impaired the input address, the harder it is to cleanse.

A cleanse that returns a match costs a lookup. A no-match response is free.

## Confidence Score

Each incorrect, missing or misspelled element subtracts from the overall confidence score.

### Deciding on an Acceptable Confidence Score Threshold

Inputs differ widely between address cleanse projects. Within a project, though, they tend to repeat the same errors. Some datasets are keyed in by hand and prone to typos. Others have a persistently missing datapoint such as organisation name or postcode. There is no absolute Confidence Score threshold. Set the acceptable score project by project, based on the systematic errors in the data and your business goals.

To set a threshold, load a subset of the dataset into a spreadsheet application like Excel and sort on the score. Scrolling from top to bottom shows matches from best to worst. As you reach the lower quality searches you can judge roughly:

- Which confidence scores indicate ambiguous matches (i.e. up to building level only)
- Which confidence scores indicate a poor or no match (i.e. the nearest matching address is too far from the input address)

Depending on your business goals, you can also use the Match Levels to determine an acceptable match. You may need to match only up to the thoroughfare or building name, or accurate organisation names may matter.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `tags` | query | no | string | A comma separated list of tags to query over. |
| `context` | query | no | string | Identify the country of the address to cleanse. Defaults to UK (GBR) |

## Request Body

Content-Type: `application/json` (required)

| Field | Required | Type | Description |
|---|---|---|---|
| `query` | yes | string | Freeform address input to cleanse |
| `postcode` | no | string | Optionally specify the postal code for the address. |
| `post_town` | no | string | Optionally specify the city or town of the address. |
| `county` | no | string | Optionally specify the county of the address. |

## Request Samples

**curl**

```bash
curl -X POST 'https://api.ideal-postcodes.co.uk/v1/cleanse/addresses' \
  -H 'Authorization: api_key="ak_test"' \
  -H 'Content-Type: application/json' \
  -d '{
    "query": "10 downing street sw1a"
  }'
```

**JavaScript**

```javascript
const response = await fetch('https://api.ideal-postcodes.co.uk/v1/cleanse/addresses', {
  method: 'POST',
  headers: {
    'Authorization': 'api_key="ak_test"',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    query: '10 downing street sw1a',
  }),
});

const { result } = await response.json();
```

**Python**

```python
import requests

response = requests.post(
    "https://api.ideal-postcodes.co.uk/v1/cleanse/addresses",
    headers={"Authorization": 'api_key="ak_test"'},
    json={
        "query": "10 downing street sw1a",
    },
)
result = response.json()["result"]
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/cleanse/addresses")
body = {
  query: "10 downing street sw1a",
}
response = Net::HTTP.post(uri, body.to_json, "Authorization" => 'api_key="ak_test"', "Content-Type" => "application/json")
result = JSON.parse(response.body)["result"]
```

**PHP**

```php
<?php
$ch = curl_init("https://api.ideal-postcodes.co.uk/v1/cleanse/addresses");
curl_setopt_array($ch, [
  CURLOPT_POST => true,
  CURLOPT_HTTPHEADER => ['Authorization: api_key="ak_test"', "Content-Type: application/json"],
  CURLOPT_POSTFIELDS => json_encode([
    "query" => "10 downing street sw1a",
  ]),
  CURLOPT_RETURNTRANSFER => true,
]);
$result = json_decode(curl_exec($ch), true)["result"];
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |
| `result` | yes | [GbrCleanseMatch](../data/gbr-cleanse-match.md) \| [GbrCleanseNoMatch](../data/gbr-cleanse-no-match.md) |  |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/address-cleanse)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
