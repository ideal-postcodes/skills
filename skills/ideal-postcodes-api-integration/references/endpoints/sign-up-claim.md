# Poll CLI Signup Token

**Endpoint:** `GET /sign_up/{cli_token}`

**Operation ID:** `SignUpClaim`

**Tags:** Sign Up

Polls the status of a previously minted `cli_token`. The CLI calls this endpoint repeatedly after directing the user to the `signup_url` returned by `POST /sign_up`.

- `202 Accepted` — Rails has not propagated the user to the api yet, or has propagated the user but not minted the first API key. Keep polling. **Note:** the api does not record the `cli_token` at mint time, so a `202` is also what an unknown / never-propagated token returns. The CLI must impose its own polling deadline.
- `200 OK` — User and first API key exist. Credentials are returned exactly once; subsequent polls return `410 Gone`.
- `410 Gone` — Token has expired (per the `cli_token_expires_at` set by Rails on the User record) or has already been claimed.

## Parameters

| Name | In | Required | Type | Description |
|---|---|---|---|---|
| `cli_token` | path | yes | string | The token returned by `POST /sign_up`. |

## Response Schema (200)

| Field | Required | Type | Description |
|---|---|---|---|
| `code` | yes | integer |  |
| `message` | yes | string |  |
| `result` | yes | object |  |

## Error Status Codes

| HTTP | Code | Message |
|---|---|---|
| 202 |  | Pending — keep polling |
| 410 |  | Signup token expired or already claimed |

## See also

- [Live docs](https://docs.ideal-postcodes.co.uk/docs/api/sign-up-claim)
- [Authentication](../authentication.md)
- [Error Codes](../error-codes.md)
- [Data Models](../data/)
