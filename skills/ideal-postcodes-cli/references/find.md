# idpc find & resolve

Address autocomplete: two-step by design. Useful when you need to pin a specific address from partial info before using it downstream.

## `idpc find <query>`

`GET /autocomplete/addresses`.

| Flag | Description |
|---|---|
| `--country <iso3>` | Limit suggestions to one country, by ISO-3 code (for example GBR, USA) |

**TTY (human):** prints a numbered list of suggestions with their ids.

**Non-TTY (agent):** emits suggestions as JSON:

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

Returns the resolved address as `result`, in UK format for addresses in any country. On a TTY, prints the address lines.

## Agent pattern

```bash
# Pick the top hit and resolve
ID=$(idpc find "10 downing" | jq -r '.suggestions[0].id')
idpc resolve "$ID"
```
