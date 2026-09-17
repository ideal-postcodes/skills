---
name: ideal-postcodes-api-integration
description: |
  Use when integrating the Ideal Postcodes API directly via HTTP.
  Covers authentication (header + query forms), the core lookup endpoints
  (postcodes, autocomplete find and resolve, places, cleanse), key/licensee/config
  admin, the Address and AddressListItem models with their `dataset` and
  `native` (raw dataset record) fields, the `context` country parameter, error
  codes, rate limits, and common gotchas around allowed URLs.
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

Direct HTTP integration with the Ideal Postcodes API. Supports multiple client libraries and language SDKs. UK and Irish addresses by default, with international coverage selected per request.

## Quick Start (fetch)

```javascript
const key = process.env.IDEAL_POSTCODES_API_KEY;
const base = 'https://api.ideal-postcodes.co.uk/v1';
const headers = { Authorization: `api_key="${key}"` };

// Postcode lookup: every address at a postcode, as an AddressListItem[]
const lookup = await fetch(`${base}/postcodes/SW1A1AA`, { headers });
const { result: addresses } = await lookup.json();
console.log(addresses[0].line_1, addresses[0].dataset); // "10 Downing Street" "paf"

// Autocomplete step 1 (free): suggestions as the user types
const query = encodeURIComponent('10 downing');
const find = await fetch(`${base}/autocomplete/addresses?query=${query}`, { headers });
const { result: { hits } } = await find.json(); // [{ id, suggestion, urls }]

// Autocomplete step 2 (one lookup): resolve a suggestion id to an Address
const id = encodeURIComponent(hits[0].id);
const resolve = await fetch(`${base}/autocomplete/addresses/${id}/gbr`, { headers });
const { result: address } = await resolve.json();
console.log(address.postcode, address.native); // native is the raw dataset record
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
console.log(data.result); // AddressListItem[]
```

## Core Endpoints

- `GET /postcodes/{postcode}` - Every address at a postcode. UK and Irish postcodes by default; set `context` to `NLD` or `SGP` for Dutch or Singapore postcodes, or `GLOBAL` to accept any. `dataset` narrows the source, `filter` trims the fields, `page` paginates the rare postcode with more than 100 premises
- `GET /autocomplete/addresses` - Typeahead address search. `context` defaults to `GBR`. Two filter families: UK (`postcode`, `postcode_outward`, `post_town`, `uprn`, `postcode_type`) and international (`postal_code`, `postal_code_2`, `postal_code_3`, `city`, `state`, `state_code`, `is_pobox`, `is_business`). Each filter has a `bias_` twin that boosts instead of restricts. At most 8 filter terms and 5 bias terms
- `GET /autocomplete/addresses/{address}/gbr` - Resolve a suggestion `id` to an `Address`. The resolved address always carries `native`
- `GET /places` / `GET /places/{place}` - Place search and resolve
- `POST /cleanse/addresses` - Match a freeform address string to a record, scored by `fit` and `confidence`
- `GET /emails`, `GET /phone_numbers` - Email and phone validation

See [`endpoints/`](./references/endpoints/) for one reference per operation.

## Address Shapes

- **`Address`** - returned by resolve, cleanse and every other single-address endpoint. Always carries `native`. See [`data/address.md`](./references/data/address.md)
- **`AddressListItem`** - returned in the arrays from `/postcodes/{postcode}`. Same fields as `Address`, but `native` is absent for the Royal Mail PAF family (`paf`, `mr`, `nyb`, `pafa`, `pafw`) and present for AddressBase and every non-UK dataset. See [`data/address-list-item.md`](./references/data/address-list-item.md)
- **`dataset`** - names the source of each address (`paf`, `abp`, `usps`, `kadaster`, ...). Read it before reading `native`
- **`native`** - the raw record from that dataset, exactly as the source supplies it. Its shape depends on `dataset`: a `paf` address carries a [`PafAddress`](./references/data/paf-address.md), a `usps` address a [`UspsAddress`](./references/data/usps-address.md), and so on. Every record type has a reference under [`data/`](./references/data/), each with a field table and an example
- **International addresses** use the UK layout: city maps to `post_town`, state or province to `county`, and up to three address lines. `udprn` and `umprn` are `0` on a UK dataset without one and `""` outside the UK; use `id` for an identifier present on every address

## Critical Gotchas

- **Auth header format** - `Authorization: api_key="<your-key>"` (with double quotes around the key). Not `Bearer`, not `ApiKey <key>`. Query-string `?api_key=...` also works
- **API key restrictions** - default keys are restricted to specific domains. Ensure your domain is in the key's allowed list
- **Rate limits** - each IP is rate limited at 30 requests per second. Tripping the limit returns a 503. Autocomplete adds its own limit of 3,000 requests per 5 minutes per key and IP, reported in the `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` headers
- **Free find, paid resolve** - autocomplete suggestions cost nothing; resolving a suggestion costs one lookup. Keys that find without ever resolving are rate limited, then suspended
- **Resolve by `id`** - a suggestion's `id` is what you pass to the resolve endpoint. Its `urls` object can be ignored
- **Unfound postcode is free** - `/postcodes/{postcode}` returns `404` with code `4040` and a `suggestions` array of up to 5 nearby postcodes. No lookup is charged
- **`native` on list endpoints** - do not rely on `native` in `/postcodes` results; it is absent for PAF-family addresses there. Resolve the address if you need the raw record
- **Error responses** - all errors are JSON with a numeric `code` and human-readable `message`. Check codes before retrying - see [`error-codes.md`](./references/error-codes.md)

## Reference Layout

- [`authentication.md`](./references/authentication.md) - header and query auth, key restrictions
- [`error-codes.md`](./references/error-codes.md) - error code catalogue with fixes
- [`endpoints/`](./references/endpoints/) - one reference per API operation: parameters, request body, request samples in curl, JavaScript, Python, Ruby and PHP, response schema, response example, status codes
- [`data/`](./references/data/) - one reference per data model (`Address`, `AddressListItem`, the cleanse match, every `native` dataset record, suggestions, key admin) with field tables, an example and "used by" cross-links

## Full Spec

The complete OpenAPI spec is available at:

- npm: `@ideal-postcodes/openapi`
- web: <https://openapi.ideal-postcodes.co.uk/openapi.yaml>

Reach for it when you need exhaustive parameter detail beyond what's in the endpoint references.

## Full documentation

The full Ideal Postcodes documentation - every guide, API reference, and integration - is available as a single file at [llms.txt](https://docs.ideal-postcodes.co.uk/llms.txt). Point your agent there for anything this skill doesn't cover.
