# `idpc auth`

Manage stored credentials at `~/.config/ideal-postcodes/credentials.json` (or `%APPDATA%\ideal-postcodes\credentials.json` on Windows, or `$XDG_CONFIG_HOME/ideal-postcodes/...` if set). File is written with mode `0600`.

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
{ "success": true, "profile": "default", "config_path": "/home/you/.config/ideal-postcodes/credentials.json" }
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
  "user_token": { "source": "config", "profile": "default", "preview": "ut_xxx…" },
  "live": true,
  "availability": { /* GET /keys/{key} response */ }
}
```

`source` is `flag`, `env`, or `config`.
