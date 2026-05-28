# `idpc phone`

Validate phone numbers — `GET /phone_numbers`. Requires `api_key`. Each input counts as a paid lookup.

## Usage

```
idpc phone [options] [query]
idpc phone --file input.txt
idpc phone --stdin
```

| Flag | Description |
|---|---|
| `-f, --file <path>` | Read one phone number per line |
| `--stdin` | Read one phone number per line from stdin |
| `-o, --out <path>` | Write CSV to a file (batch mode only) |
| `--carrier` | Look up the current network carrier (slower) |

Exactly one of: `[query]`, `--file`, `--stdin`. Requests are tagged `cli`.

Include the country code (e.g. `+442071128019`) for reliable parsing.

## Output

**Single input** (always JSON):

```json
{
  "query": "+442071128019",
  "response": {
    "result": {
      "valid": true,
      "national_format": "020 7112 8019",
      "international_format": "+44 20 7112 8019",
      "iso_country": "GBR",
      "iso_country_2": "GB",
      "country": "United Kingdom",
      "current_carrier": { "name": null },
      "original_carrier": { "name": "Invomo Ltd", "network_type": "landline" }
    }
  }
}
```

`current_carrier` is only populated when `--carrier` is passed (an HLR lookup that is slower and may cost more). Unparseable numbers return `"valid": false` with the format fields blank.

**Multiple inputs** (file or stdin) emit CSV by default; pass `--json` for the
`{ count, results: [{ query, response }] }` shape. CSV columns (carriers
flattened to their name):

```
query,status,valid,national_format,international_format,iso_country,iso_country_2,country,current_carrier,original_carrier
```

A row that fails its lookup doesn't abort the batch: in CSV its `status`
becomes `Error: <message>`; in JSON its entry is `{ query, error: { code, message } }`.
The batch still exits `0` — filter on `status` / the `error` key to find failures.
Auth/permission failures (`auth_failed`, `forbidden`) are the exception: they
abort the whole run since every row would fail identically.

## Agent patterns

```bash
# Single — is it valid?
idpc phone "+442071128019" | jq .response.result.valid

# Batch to CSV
cat numbers.txt | idpc phone --stdin > numbers.csv

# Filter invalid numbers from a batch
cat numbers.txt | idpc phone --stdin --json | jq '.results[] | select(.response.result.valid == false) | .query'
```

## Error codes

- `missing_argument` — no `[query]`, `--file`, or `--stdin` supplied
- `auth_failed` — invalid key, or key lacks paid phone validation permission
- `rate_limited` — back off and retry
- `invalid_input` — malformed request rejected by the API
