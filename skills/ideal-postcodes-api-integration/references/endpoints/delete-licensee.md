# Cancel Licensee

**Endpoint:** `DELETE /keys/{key}/licensees/{licensee}`

**Operation ID:** `DeleteLicensee`

**Tags:** Licensees

Cancels a licensee. Its key stops working and it drops out of the licensee list. Contact us to reverse it.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `key` | path | yes | string | The API Key. Begins `ak_`. |
| `licensee` | path | yes | string | Uniquely identifies a licensee. |
| `user_token` | query | no | string | A secret key used to manage your account and API Keys. It was previously called the user token. |

## Request Samples

**curl**

```bash
curl -X DELETE 'https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret'
```

**JavaScript**

```javascript
import { createIdpcClient, deleteLicensee } from '@ideal-postcodes/sdk';

const client = createIdpcClient({ apiKey: 'ak_test', userToken: 'uk_secret' });

const { data } = await deleteLicensee({
  client,
  path: { key: 'ak_test', licensee: 'sl_ijoiqsxeQgXW2gkiE0X94' },
});
```

**Python**

```python
# pip install ideal-postcodes
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    result = client.delete_licensee("ak_test", "sl_ijoiqsxeQgXW2gkiE0X94", user_token="uk_secret")
```

**Ruby**

```ruby
require "net/http"

uri = URI("https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret")
Net::HTTP.start(uri.host, use_ssl: true) do |http|
  http.delete(uri.request_uri)
end
```

**PHP**

```php
<?php
$ch = curl_init("https://api.ideal-postcodes.co.uk/v1/keys/ak_test/licensees/sl_ijoiqsxeQgXW2gkiE0X94?user_token=uk_secret");
curl_setopt_array($ch, [
  CURLOPT_CUSTOMREQUEST => "DELETE",
  CURLOPT_RETURNTRANSFER => true,
]);
curl_exec($ch);
```

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `result` | yes | object |  |
| `code` | yes | `2000` |  |
| `message` | yes | `Success` |  |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/delete-licensee)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
