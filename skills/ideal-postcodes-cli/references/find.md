# `idpc find` & `idpc resolve`

Address autocomplete — two-step by design. Useful when you need to pin a specific address from partial info before using it downstream.

## `idpc find [query]`

`GET /autocomplete/addresses`.

| Flag | Description |
|---|---|
| `--country <iso3>` | Filter by ISO-3 (e.g. GBR, USA) |

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

## `idpc resolve <id>`

`GET /autocomplete/addresses/{id}/gbr`.

Returns the resolved address object.

## Agent pattern

```bash
# Pick the top hit and resolve
ID=$(idpc find "10 downing" | jq -r '.suggestions[0].id')
idpc resolve "$ID"
```
