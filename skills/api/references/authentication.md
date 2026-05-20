# Authentication

Every API request needs an API key. Keys are minted in the dashboard at <https://ideal-postcodes.co.uk/account>. Test keys (e.g. `ak_test`) have free balance and bypass URL restrictions.

## Header (recommended for server-side)

```
Authorization: api_key="ak_yourkey"
```

The key value **must be wrapped in double quotes**. `Authorization: ApiKey ak_yourkey` and `Authorization: Bearer ak_yourkey` are both rejected.

Multiple parameters can be combined in the same header, comma-separated:

```
Authorization: api_key="ak_yourkey", user_token="..."
```

## Query string

```
GET /v1/postcodes/SW1A1AA?api_key=ak_yourkey
```

Use the query form when calling from the browser via a JSONP-style integration or when the request runs through a redirect that strips headers. Both forms accept the same key; pick one per request — they don't merge.

## Key restrictions

Live keys are typically locked down with one or both of:

- **Allowed URLs** — the request's `Origin` or `Referer` header must match an entry on the key's allowlist. Server-side requests don't send these headers and so bypass the check; browser requests must use a key with the calling page's URL on the allowlist
- **Daily lookup limit** — caps per-day spend at the configured threshold

`localhost` is allowed by default for development. Production domains must be added explicitly.

A request rejected by Allowed URL matching returns HTTP 401 with code `4011` (URL not on whitelist). See [`error-codes.md`](./error-codes.md).

## Inspecting and managing keys

The Keys endpoints let you check balance, usage, and configuration without the dashboard:

- [`endpoints/key-availability.md`](./endpoints/key-availability.md) — is the key live and within its limits
- [`endpoints/key-details.md`](./endpoints/key-details.md) — full configuration
- [`endpoints/key-usage.md`](./endpoints/key-usage.md) — usage stats
- [`endpoints/key-logs.md`](./endpoints/key-logs.md) — request logs

Mutating key details (allowed URLs, limits) requires a `user_token` — your account's secret key, separate from `api_key`. Available from the dashboard under account settings.

## Test keys

`ak_test` is the canonical test key. It always returns the same canned responses (`SW1A 2AA` → 10 Downing Street, etc.) and never consumes balance. Use it in integration tests and CI; switch to your live key for production.
