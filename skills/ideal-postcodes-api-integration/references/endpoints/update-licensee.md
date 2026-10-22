# Update Licensee

**Endpoint:** `POST /keys/{key}/licensees/{licensee}`

**Operation ID:** `UpdateLicensee`

**Tags:** Licensees

Updates a licensee's address, postcode, allowed URLs and daily limit. Returns the updated licensee. The name is fixed at creation.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `key` | path | yes | string | The API Key. Begins `ak_`. |
| `licensee` | path | yes | string | Uniquely identifies a licensee. |
| `user_token` | query | no | string | A secret key used to manage your account and API Keys. It was previously called the user token. |

## Request Body

Content-Type: `application/json` (required)

| Field | Required | Type | Description |
|---|---|---|---|
| `name` | no | string | Licensee individual or organisation name |
| `address` | no | string | Licensee's first, second and third line address as well as post town concatenated by commas |
| `postcode` | no | string | Licensee's postcode |
| `whitelist` | no | array<string> | A list of allowed URLs. An empty list disables the check. |
| `daily` | no | object |  |

## Request Samples

**curl**

```bash
curl -X POST 'https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret' \
  -H 'Content-Type: application/json' \
  -d '{
    "daily": {
      "limit": 20000
    }
  }'
```

**JavaScript**

```javascript
import { createIdpcClient, updateLicensee } from '@ideal-postcodes/sdk';

const client = createIdpcClient({ apiKey: 'ak_test', userToken: 'uk_secret' });

const { data } = await updateLicensee({
  client,
  path: { key: 'ak_test', licensee: 'sl_ijoiqsxeQgXW2gkiE0X94' },
  body: {
    daily: {
      limit: 20000,
    },
  },
});
```

**Python**

```python
# pip install ideal-postcodes
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.update_licensee(
        "ak_test",
        "sl_ijoiqsxeQgXW2gkiE0X94",
        user_token="uk_secret",
        daily={"limit": 20000},
    )
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret")
body = {
  daily: {
    limit: 20000,
  },
}
response = Net::HTTP.post(uri, body.to_json, "Content-Type" => "application/json")
result = JSON.parse(response.body)["result"]
```

**PHP**

```php
<?php
$ch = curl_init("https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret");
curl_setopt_array($ch, [
  CURLOPT_CUSTOMREQUEST => "POST",
  CURLOPT_HTTPHEADER => ["Content-Type: application/json"],
  CURLOPT_POSTFIELDS => json_encode([
    "daily" => [
      "limit" => 20000,
    ],
  ]),
  CURLOPT_RETURNTRANSFER => true,
]);
$result = json_decode(curl_exec($ch), true)["result"];
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `result` | yes | [Licensee](../data/licensee.md) |  |
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/update-licensee)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
