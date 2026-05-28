# `idpc email`

Validate email addresses — `GET /emails`. Requires `api_key`. Each input counts as a paid lookup.

## Usage

```
idpc email [options] [query]
idpc email --file input.txt
idpc email --stdin
```

| Flag | Description |
|---|---|
| `-f, --file <path>` | Read one email per line |
| `--stdin` | Read one email per line from stdin |
| `-o, --out <path>` | Write CSV to a file (batch mode only) |

Exactly one of: `[query]`, `--file`, `--stdin`. Requests are tagged `cli`.

## Output

**Single input** (always JSON):

```json
{
  "query": "support@example.com",
  "response": {
    "result": {
      "result": "deliverable",
      "deliverable": true,
      "disposable": false,
      "free": false,
      "role": true,
      "catchall": false,
      "suggestions": []
    }
  }
}
```

`result` is `"deliverable"` or `"not_deliverable"`. `suggestions` lists corrected spellings (e.g. a fixed typo'd domain).

**Multiple inputs** (file or stdin) emit CSV by default; pass `--json` for the
`{ count, results: [{ query, response }] }` shape. CSV columns:

```
query,status,result,deliverable,disposable,free,role,catchall,suggestions
```

A row that fails its lookup doesn't abort the batch: in CSV its `status`
becomes `Error: <message>`; in JSON its entry is `{ query, error: { code, message } }`.
The batch still exits `0` — filter on `status` / the `error` key to find failures.
Auth/permission failures (`auth_failed`, `forbidden`) are the exception: they
abort the whole run since every row would fail identically.

## Agent patterns

```bash
# Single — is it deliverable?
idpc email "support@example.com" | jq -r .response.result.result

# Batch to CSV
cat emails.txt | idpc email --stdin > emails.csv

# Batch as JSON
cat emails.txt | idpc email --stdin --json | jq '.results[] | select(.response.result.result != "deliverable")'
```

## Error codes

- `missing_argument` — no `[query]`, `--file`, or `--stdin` supplied
- `auth_failed` — invalid key, or key lacks paid email validation permission
- `rate_limited` — back off and retry
- `invalid_input` — malformed request rejected by the API
