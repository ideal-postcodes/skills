# `idpc auth`

Manage stored credentials at `~/.config/ideal-postcodes/credentials.json` (or `%APPDATA%\ideal-postcodes\credentials.json` on Windows, or `$XDG_CONFIG_HOME/ideal-postcodes/...` if set). File is written with mode `0600`.

## `idpc auth signup`

Mint a one-shot `cli_token`, print a prefilled signup URL, then poll the api until the new account propagates. On success, the test API key + user token are written to the same store `idpc auth login` uses.

The command **pauses for a human step**: someone must open the printed URL in a browser and complete the captcha + ToS. The terminal resumes automatically when Rails finishes propagating the user.

| Flag | Description |
|---|---|
| `--email <email>` | Account holder email (required in non-interactive mode) |
| `--name <name>` | Account holder full name (required in non-interactive mode) |
| `--org-name <name>` | Organisation name (required) |
| `--org-address-line-one <line>` | Organisation address line 1 (required) |
| `--org-address-line-two <line>` | Optional address line 2 |
| `--org-address-line-three <line>` | Optional address line 3 |
| `--org-post-town <town>` | Organisation post town (required) |
| `--org-postcode <postcode>` | Organisation postcode (required) |
| `--org-country-code <iso2>` | Organisation country code, ISO 3166-1 alpha-2 (required) |

Polling runs every 3 seconds and gives up after 30 minutes — both are fixed. Override the api host (rarely needed) with the global `--base-url`.

Non-interactive (no TTY or `--json`): every required flag must be present; missing one returns `invalid_input` and exits 2. Interactive: prompts for missing required flags.

### Progress signals (JSON / non-TTY mode)

NDJSON events on **stderr** so the final result on stdout stays a single JSON document:

```jsonl
{"event":"signup_url_issued","signup_url":"https://ideal-postcodes.co.uk/users/sign_up?...","expires_at":"2026-05-18T18:30:00Z"}
{"event":"waiting","elapsed_sec":30}
```

### Success output (stdout)

```json
{
  "success": true,
  "config_path": "/home/you/.config/ideal-postcodes/credentials.json",
  "email": "you@example.com",
  "user_id": "..."
}
```

### Exit codes

- `0` — success
- `2` — `invalid_input` (bad flags or 400 from api), `rate_limited` (429; not retried)
- `3` — `network_error`
- `4` — `signup_expired` (410 from api) or `timeout` (polling ceiling reached)
- `130` — SIGINT (re-run the command to resume; the existing link is not recoverable)

### Example

```bash
idpc auth signup \
  --email you@example.com \
  --name "Your Name" \
  --org-name "Your Company Ltd" \
  --org-address-line-one "1 Example Street" \
  --org-post-town London \
  --org-postcode SW1A1AA \
  --org-country-code GB
```

## `idpc auth login`

Store an `api_key` and `user_token` for later reuse.

| Flag | Description |
|---|---|
| `--api-key <k>` | API key to store (required in non-interactive mode) |
| `--user-token <t>` | User token to store (required in non-interactive mode) |

Non-interactive (no TTY or `--json`): both flags required; missing either returns `invalid_input`.

Interactive: prompts for both via masked password input.

### Output

```json
{ "success": true, "config_path": "/home/you/.config/ideal-postcodes/credentials.json" }
```

### Error codes

- `invalid_input` — non-interactive mode missing `--api-key` or `--user-token`
- `write_failed` — unable to write credentials file

## `idpc auth logout`

Remove the credentials file.

### Output

```json
{ "success": true, "removed": true, "config_path": "..." }
```

`removed: false` means there was no file to remove — not an error.

## `idpc auth whoami`

Show where each credential was resolved from and call `GET /keys/{key}` to verify the api_key is live.

### Output

```json
{
  "api_key": { "source": "env", "preview": "ak_xxx…" },
  "user_token": { "source": "config", "preview": "ut_xxx…" },
  "live": true,
  "availability": { /* GET /keys/{key} response */ }
}
```

`source` is `flag`, `env`, or `config`.
