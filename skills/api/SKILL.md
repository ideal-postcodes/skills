---
name: api
description: |
  Use when integrating the Ideal Postcodes API directly via HTTP.
  Covers authentication (header + query forms), the core lookup endpoints
  (postcodes, autocomplete, places, cleanse), key/licensee/config admin,
  data models (PafAddress, MrAddress, AddressSuggestion, etc.), error
  codes, rate limits, and common gotchas around CORS and allowed URLs.
license: SEE LICENSE IN LICENSE
metadata:
  author: ideal-postcodes
  version: "0.1.0"
  homepage: https://ideal-postcodes.co.uk
  source: https://github.com/ideal-postcodes/skills
  openclaw:
    primaryEnv: IDEAL_POSTCODES_API_KEY
    envVars:
      - name: IDEAL_POSTCODES_API_KEY
        required: true
        description: API key. Get one at ideal-postcodes.co.uk/account
inputs:
  - name: IDEAL_POSTCODES_API_KEY
    description: API key for the Ideal Postcodes API
    required: true
references:
  - authentication.md
  - error-codes.md
  - endpoints/
  - data/
---

# Ideal Postcodes API

Direct HTTP integration with the Ideal Postcodes API. Supports multiple client libraries and language SDKs.

## Quick Start (fetch)

```javascript
const key = process.env.IDEAL_POSTCODES_API_KEY;
const response = await fetch(
  'https://api.ideal-postcodes.co.uk/v1/postcodes/SW1A1AA',
  {
    headers: {
      Authorization: `api_key="${key}"`,
    },
  }
);

const data = await response.json();
console.log(data.result); // Array of addresses
```

Alternatively, pass `?api_key=...` in the query string.

## Quick Start (axios)

```javascript
import axios from 'axios';

const client = axios.create({
  baseURL: 'https://api.ideal-postcodes.co.uk/v1',
  headers: {
    Authorization: `api_key="${process.env.IDEAL_POSTCODES_API_KEY}"`,
  },
});

const { data } = await client.get('/postcodes/SW1A1AA');
console.log(data.result);
```

## Core Endpoints

- `GET /postcodes/{postcode}` — Lookup all addresses for a UK postcode
- `GET /autocomplete/addresses` — Typeahead address search
- `GET /autocomplete/addresses/{address}/gbr` — Resolve a UK autocomplete suggestion
- `GET /places` / `GET /places/{place}` — Place search and resolve
- `POST /cleanse/addresses` — Cleanse a freeform address string
- `GET /emails`, `GET /phone_numbers` — Email and phone validation

See [`endpoints/`](./references/endpoints/) for the full list (23 operations).

## Critical Gotchas

- **Auth header format** — `Authorization: api_key="<your-key>"` (with double quotes around the key). Not `Bearer`, not `ApiKey <key>`. Query-string `?api_key=...` also works
- **API key restrictions** — default keys are restricted to specific domains. Ensure your domain is in the key's allowed list
- **CORS in browsers** — the API sets CORS headers for `http://localhost` in development. Production domains must be explicitly added to your key
- **Rate limits** — each IP is rate limited at 30 requests per second. Tripping the limit returns a 503
- **Error responses** — all errors are JSON with a numeric `code` and human-readable `message`. Check codes before retrying — see [`error-codes.md`](./references/error-codes.md)

## Reference Layout

- [`authentication.md`](./references/authentication.md) — header and query auth, key restrictions
- [`error-codes.md`](./references/error-codes.md) — error code catalogue with fixes
- [`endpoints/`](./references/endpoints/) — one reference per API operation (parameters, request body, response schema, examples, status codes)
- [`data/`](./references/data/) — one reference per data model (PafAddress, MrAddress, AddressSuggestion, etc.) with field tables and "used by" cross-links

## Full Spec

The complete OpenAPI spec is available at:

- npm: `@ideal-postcodes/openapi`
- web: <https://openapi.ideal-postcodes.co.uk/openapi.yaml>

Reach for it when you need exhaustive parameter detail beyond what's in the endpoint references.
