---
name: ideal-postcodes-cli
description: >
  Drive api.ideal-postcodes.co.uk from the terminal — manage API keys
  (balance, usage, allowed-URL configs, lookup logs), cleanse messy addresses,
  validate email addresses and phone numbers, and resolve specific addresses
  from partial queries via the `idpc` CLI.
  Use when the user wants to inspect or update an Ideal Postcodes account,
  cleanse addresses in bulk, validate emails or phone numbers, or pin down a
  specific address from a partial query. Always load this skill before running
  `idpc` — it defines the
  non-interactive flag contract and output shape that keep agent runs
  deterministic.
license: SEE LICENSE IN LICENSE
metadata:
  author: ideal-postcodes
  source: https://github.com/ideal-postcodes/atlas/tree/main/packages/cli-ideal
  envVars:
    - name: IDPC_API_KEY
      required: true
      description: Ideal Postcodes API key
    - name: IDPC_USER_TOKEN
      required: false
      description: Required for private key endpoints (details, usage, logs, configs)
references:
  - auth.md
  - keys.md
  - cleanse.md
  - email.md
  - phone.md
  - find.md
---

# Ideal Postcodes CLI (`idpc`)

## Installation

Requires Node.js 22 or later.

```bash
npm install -g @ideal-postcodes/cli
idpc --version
```

## Agent Protocol

The CLI auto-detects non-TTY environments and emits JSON — no `--json` flag needed when piping or running headless.

**Rules for agents:**

- Supply ALL required flags. The CLI will NOT prompt when stdin is not a TTY.
- `-q / --quiet` suppresses any status output and implies `--json`.
- Exit `0` = success, `1` = error.
- Both success and error JSON go to **stdout** — parse it uniformly, then check for an `error` key and the exit code:
  ```json
  {"error":{"code":"...","message":"...","details":{...}}}
  ```
- Destructive commands (e.g. `keys configs delete`) require `--yes` in non-TTY.
- Use env vars or flags. Never rely on `idpc auth login` from an agent.

## Authentication

Two credentials: `api_key` (required) and `user_token` (required for `/keys/*` reads, configs, updates). Resolution precedence — per credential:

| Priority | Source |
|---|---|
| 1 (highest) | `--api-key <k>` / `--user-token <t>` |
| 2 | `IDPC_API_KEY` / `IDPC_USER_TOKEN` env var |
| 3 (lowest) | `~/.config/ideal-postcodes/credentials.json` (written by `idpc auth login`) |

Missing api_key → error code `missing_api_key`. Missing user_token on a command that needs it → `missing_user_token`.

## Global Flags

| Flag | Description |
|------|-------------|
| `--api-key <k>` | Override API key for this invocation |
| `--user-token <t>` | Override user token |
| `--json` | Force JSON (auto in non-TTY) |
| `-q, --quiet` | Suppress status, implies `--json` |
| `--base-url <url>` | Override API base (diagnostics only) |

## Available Commands

| Group | Subcommands |
|---|---|
| `idpc auth` | `login`, `logout`, `whoami` |
| `idpc keys` | `get`, `details`, `update`, `usage`, `logs`, `configs {list,get,create,update,delete}` |
| `idpc cleanse` | Cleanse one address, a file, or stdin |
| `idpc email` | Validate one email, a file, or stdin |
| `idpc phone` | Validate one phone number, a file, or stdin |
| `idpc find` / `resolve` | Autocomplete then resolve a suggestion id to a full address (paired; see `find.md`) |
| `idpc doctor` | Env + connectivity check |

Read the matching reference file for flags and example output.

## Common Pitfalls

- **`user_token` is separate from `api_key`.** `keys details`, `keys usage`, `keys logs`, `keys update`, and all `configs` writes require both.
- **`cleanse`, `email`, and `phone` cost paid lookups.** Public test key `iddqd` will return `auth_failed`.
- **`cleanse` / `email` / `phone` batch mode emits CSV** unless `--json` is passed; a single query always emits JSON.
- **`find` without a query in non-TTY errors.** Always pass a query when scripting.
- **`keys logs` emits raw CSV**, not JSON, and rejects `--json` / `-q` with `invalid_input`. Redirect to a file or pipe into your CSV tooling.
- **Credentials file is `0600`.** If your umask is unusual, `idpc auth login` may fail with `write_failed`.

## Quick Examples

```bash
# Cleanse one address (JSON to stdout)
idpc cleanse "10 downing street, london"

# Batch cleanse
cat addresses.txt | idpc cleanse --stdin

# Validate an email or phone number
idpc email "support@example.com"
idpc phone "+442071128019" --carrier

# Batch validate emails to a CSV
cat emails.txt | idpc email --stdin > emails.csv

# Find a specific address and resolve it
ID=$(idpc find "10 downing" | jq -r '.suggestions[0].id')
idpc resolve "$ID"

# Inspect a key
idpc keys details

# Add an allowed URL
idpc keys configs update web --allowed-urls https://example.com,https://www.example.com
```

## Full documentation

The full Ideal Postcodes documentation — every guide, API reference, and integration — is available as a single file at [llms.txt](https://docs.ideal-postcodes.co.uk/llms.txt). Point your agent there for anything this skill doesn't cover.
