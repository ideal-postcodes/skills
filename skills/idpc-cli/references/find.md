# `idpc find` & `idpc resolve`

Address autocomplete — two-step by design. Useful when you need to pin a specific address from partial info before using it downstream.

## `idpc find [query]`

`GET /autocomplete/addresses`.

| Flag | Description |
|---|---|
| `--country <iso3>` | Filter by ISO-3 (e.g. GBR, USA) |
| `--resolve` | In non-TTY, auto-call resolve on each suggestion |

**TTY (human):** prompts for input if no query, shows a `select` picker, auto-resolves the chosen suggestion, prints the full address.

**Non-TTY (agent):** query is required. Emits suggestions as JSON:

```json
{
  "count": 2,
  "suggestions": [
    { "id": "ABC123", "suggestion": "10 Downing Street, London, SW1A" },
    { "id": "DEF456", "suggestion": "11 Downing Street, London, SW1A" }
  ]
}
```

Adding `--resolve` pairs each suggestion with its full address in one call:

```json
{
  "count": 2,
  "results": [
    { "suggestion": { ... }, "address": { /* full */ } },
    { "suggestion": { ... }, "address": { /* full */ } }
  ]
}
```

## `idpc resolve <id>`

`GET /autocomplete/addresses/{id}/{country}`.

| Flag | Description |
|---|---|
| `--country <iso3>` | ISO-3 country code (default: `gbr`) |

Returns the resolved address object.

## Agent pattern

```bash
# Pick the top hit and resolve
ID=$(idpc find "10 downing" | jq -r '.suggestions[0].id')
idpc resolve "$ID"

# Or in one shot
idpc find "10 downing" --resolve | jq '.results[0].address'
```
