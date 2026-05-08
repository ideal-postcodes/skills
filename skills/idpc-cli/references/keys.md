# `idpc keys`

Inspect and manage API keys.

Every subcommand accepts `[key]` as an optional first positional. If omitted, the resolved api_key is used — so `idpc keys details` and `idpc keys details ak_xxx` behave the same when your own key is configured.

## `idpc keys get [key]`

Public availability info — `GET /keys/{key}`. No user_token required.

## `idpc keys details [key]`

Private key details — `GET /keys/{key}/details`. **Requires user_token.**

## `idpc keys update [key]`

Update key details — `PUT /keys/{key}/details`. **Requires user_token.**

| Flag | Description |
|---|---|
| `--notify-email <email>` | Set the notification email |
| `--automated-topup <bool>` | `true` or `false` |
| `--json-body <json>` | Raw JSON merged last — use for fields not covered above |

Must provide at least one field.

## `idpc keys usage [key]`

Lookup counts over a date range — `GET /keys/{key}/usage`. **Requires user_token.**

| Flag | Description |
|---|---|
| `--start <date>` | Start date (YYYY-MM-DD or ISO) |
| `--end <date>` | End date |
| `--tags <csv>` | Comma-separated tag filter |
| `--licensee <id>` | Filter by licensee id |

## `idpc keys logs [key]`

Paid lookup logs — `GET /keys/{key}/lookups`. Emits **raw CSV to stdout**. **Requires user_token.**

| Flag | Description |
|---|---|
| `--start <date>` | Start date |
| `--end <date>` | End date |
| `--licensee <id>` | Filter by licensee id |

Example:
```bash
idpc keys logs --start 2026-01-01 --end 2026-01-31 > lookups.csv
```

## `idpc keys configs` — allowed-URL configs

All subcommands **require user_token** (except `get`).

### `list [key]`

`GET /keys/{key}/configs` — list all configs.

### `get <config> [key]`

`GET /keys/{key}/configs/{config}` — retrieve one.

### `create <config> [key]`

`POST /keys/{key}/configs` — create a new config.

| Flag | Description |
|---|---|
| `--allowed-urls <csv>` | Comma-separated URLs |
| `--json-body <json>` | Raw JSON merged into body |

### `update <config> [key]`

`POST /keys/{key}/configs/{config}` — update in place.

| Flag | Description |
|---|---|
| `--allowed-urls <csv>` | Replace allowed URLs |
| `--json-body <json>` | Raw JSON merged |

Must pass at least one.

### `delete <config> [key]`

`DELETE /keys/{key}/configs/{config}`. Requires `--yes` in non-TTY.

| Flag | Description |
|---|---|
| `-y, --yes` | Skip confirmation |
