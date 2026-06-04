# `idpc cleanse`

Clean messy address strings — `POST /cleanse/addresses`. Requires `api_key`. Each input counts as a paid lookup.

## Usage

```
idpc cleanse [options] [query]
idpc cleanse --file input.txt
idpc cleanse --stdin
```

| Flag | Description |
|---|---|
| `-f, --file <path>` | Read one address per line |
| `--stdin` | Read one address per line from stdin |
| `-o, --out <path>` | Write CSV to a file (batch mode only) |
| `--context <iso3>` | ISO-3 country hint (GBR, USA, …) |

Exactly one of: `[query]`, `--file`, `--stdin`. Requests are tagged `cli`.

## Output

**Single input:**

```json
{
  "query": "10 downing street",
  "response": { /* IDPC cleanse response */ }
}
```

**Multiple inputs** (file or stdin):

```json
{
  "count": 3,
  "results": [
    { "query": "...", "response": { ... } },
    { "query": "...", "response": { ... } },
    { "query": "...", "response": { ... } }
  ]
}
```

Each `response` includes the IDPC `result` array and the confidence score. There is no absolute confidence threshold — decide per your dataset.

Multiple inputs emit CSV by default; pass `--json` for the shape above. A row
that fails its lookup doesn't abort the batch: in CSV its `status` becomes
`Error: <message>`; in JSON its entry is `{ query, error: { code, message } }`.
The batch still exits `0`. Auth/permission failures (`auth_failed`,
`forbidden`) are the exception: they abort the whole run.

## Agent patterns

```bash
# Single
idpc cleanse "10 downing street, london" | jq .response.result[0]

# Batch
cat messy.txt | idpc cleanse --stdin > clean.json
```

## Error codes

- `missing_argument` — no `[query]`, `--file`, or `--stdin` supplied
- `auth_failed` — invalid key, or key lacks paid cleanse permission
- `rate_limited` — 30 req/sec per IP; back off
- `invalid_input` — malformed body rejected by the API
