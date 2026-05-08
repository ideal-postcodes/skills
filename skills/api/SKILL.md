---
name: api
description: |
  Use when integrating the Ideal Postcodes API directly via HTTP or the provided SDKs.
  Covers authentication, key HTTP endpoints (/postcodes, /addresses, /udprn, /autocomplete),
  client libraries (fetch, axios, @ideal-postcodes/core-axios, Python SDK), error handling,
  rate limits, and common gotchas (API key restrictions, CORS, JSONP legacy mode, async/await
  patterns).
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
  - endpoints.md
  - sdk-node.md
  - sdk-browser.md
  - error-codes.md
  - rate-limits.md
  - openapi.yaml
---

# Ideal Postcodes API

Direct HTTP integration with the Ideal Postcodes API. Supports multiple client libraries and language SDKs.

## Quick Start (fetch)

```javascript
const response = await fetch(
  'https://api.ideal-postcodes.co.uk/postcodes/SW1A1AA',
  {
    headers: {
      'Authorization': `ApiKey ${process.env.IDEAL_POSTCODES_API_KEY}`,
    },
  }
);

const data = await response.json();
console.log(data.result); // Array of addresses
```

## Quick Start (axios)

```javascript
import axios from 'axios';

const client = axios.create({
  baseURL: 'https://api.ideal-postcodes.co.uk',
  headers: {
    'Authorization': `ApiKey ${process.env.IDEAL_POSTCODES_API_KEY}`,
  },
});

const { data } = await client.get('/postcodes/SW1A1AA');
console.log(data.result); // Array of addresses
```

## Quick Start (Node.js SDK)

```javascript
import { IdealPostcodes } from 'ideal-postcodes';

const client = new IdealPostcodes({
  apiKey: process.env.IDEAL_POSTCODES_API_KEY,
});

const addresses = await client.postcodes.lookup('SW1A1AA');
console.log(addresses);
```

## Core Endpoints

- `GET /postcodes/:postcode` — Lookup all addresses for a UK postcode
- `GET /addresses` — Search for addresses by query string
- `GET /udprn/:udprn` — Lookup a specific address by UDPRN (unique delivery point reference number)
- `GET /autocomplete` — Typeahead search for addresses

## Critical Gotchas

- **Authentication header format** — must be `Authorization: ApiKey <your-api-key>`, not `Bearer` or `X-Api-Key`
- **API key restrictions** — default keys are restricted to specific domains. Ensure your domain is in the key's allowed list
- **CORS in browsers** — the API sets CORS headers for `http://localhost` in development. Production domains must be explicitly added to your key
- **Rate limits** — keys have rate limits. See [`rate-limits.md`](./references/rate-limits.md)
- **Error responses** — all errors are JSON with an `error` object containing `code` and `message`. Check error codes before retrying

See [`./references/`](./references/) for detailed guides on authentication, endpoints, SDKs, and error handling.
