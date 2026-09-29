# Logs (CSV)

**Endpoint:** `GET /keys/{key}/lookups`

**Operation ID:** `KeyLogs`

**Tags:** Keys

Returns a CSV of the paid lookups made on a key, with the information recorded against each one.

This method requires your Management Key, which can be found on your [accounts page](https://account.ideal-postcodes.co.uk/account).

You can request a maximum interval of 90 days. Without a start or end date, the interval defaults to the last 21 days.

The `Content-Type` returned is CSV (text/csv). For a non-200 response it reverts to JSON, with the error code and message in the body.

## CSV Format

The CSV has no header row. Columns, in order:

1. Timestamp (ISO 8601)
2. IP address the request was received from
3. Search term
4. URL the request originated from
5. Lookup type
6. Tags
7. Lookups consumed
8. Licensee name (sublicensing keys only)
9. Source IP address

The source IP column carries the address forwarded in the `IDPC-Source-IP` header. It is only recorded for keys with IP address forwarding enabled, and only when the header holds a valid IP address. It is empty otherwise.

## Data Redaction

We redact Personally Identifiable Data (PII) in your usage log (including IP, source IP, search term and URL data) weekly.

By default we redact PII older than 28 days. You can change this period from your dashboard.

Set the interval to `0` days to prevent PII collection altogether.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `key` | path | yes | string | The API Key to retrieve. Begins `ak_`. |
| `user_token` | query | no | string | A secret key used to manage your account and API Keys. It was previously called the user token. |
| `start` | query | no | integer | A start date/time in the form of a UNIX Timestamp in milliseconds. E.g. `1418556452651` |
| `end` | query | no | integer | An end date/time in the form of a UNIX Timestamp in milliseconds. E.g.  `1418556477882` |
| `licensee` | query | no | string | Uniquely identifies a licensee. |

## Request Samples

**curl**

```bash
curl -G 'https://api.ideal-postcodes.co.uk/v1/keys/ak_test/lookups' \
  -d 'user_token=uk_secret'
```

**JavaScript**

```javascript
const response = await fetch(
  'https://api.ideal-postcodes.co.uk/v1/keys/ak_test/lookups?' +
  new URLSearchParams({
    user_token: 'uk_secret',
  })
);

const { result } = await response.json();
```

**Python**

```python
import requests

response = requests.get(
    "https://api.ideal-postcodes.co.uk/v1/keys/ak_test/lookups",
    params={
        "user_token": "uk_secret",
    },
)
result = response.json()["result"]
```

**Ruby**

```ruby
require "net/http"
require "json"

uri = URI("https://api.ideal-postcodes.co.uk/v1/keys/ak_test/lookups")
uri.query = URI.encode_www_form(user_token: "uk_secret")
result = JSON.parse(Net::HTTP.get(uri))["result"]
```

**PHP**

```php
<?php
$response = file_get_contents(
  "https://api.ideal-postcodes.co.uk/v1/keys/ak_test/lookups?" .
  http_build_query([
    "user_token" => "uk_secret",
  ])
);
$result = json_decode($response, true)["result"];
```

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/key-logs)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
