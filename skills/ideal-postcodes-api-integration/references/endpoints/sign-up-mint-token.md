# Mint CLI Signup Token

**Endpoint:** `POST /sign_up`

**Operation ID:** `SignUpMintToken`

**Tags:** Sign Up

Mints a one-shot `cli_token` and returns a prefilled accounts web URL where the user completes captcha + Terms of Service and Devise account creation. The CLI then polls `GET /sign_up/{cli_token}` until the api receives the user record (via the existing Rails → api admin propagation) along with the first API key, at which point credentials are returned exactly once.

The api does **not** persist the `cli_token` at mint time — the token only becomes known to the api once Rails propagates the new user. The CLI is therefore responsible for its own polling deadline; if the user never completes signup, the api has no record of this token and `GET /sign_up/{cli_token}` will keep returning `202 Accepted` indefinitely.

## Request Body

Content-Type: `application/json` (required)

| Field | Required | Type | Description |
|---|---|---|---|
| `email` | yes | string | Account holder's email address. |
| `name` | yes | string | Account holder's full name. |
| `org_name` | yes | string | Organisation name. |
| `org_address_line_one` | yes | string |  |
| `org_address_line_two` | no | string |  |
| `org_address_line_three` | no | string |  |
| `org_post_town` | yes | string |  |
| `org_postcode` | yes | string |  |
| `org_country_code` | yes | string | ISO 3166-1 alpha-2 country code. |

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `code` | yes | integer |  |
| `message` | yes | string |  |
| `result` | yes | object |  |

## Error Status Codes

| HTTP | Code | Message |
|---|---|---|
| 400 |  | Bad Request |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/sign-up-mint-token)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
