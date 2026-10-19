---
name: ideal-postcodes-python-sdk
description: Use when integrating the Ideal Postcodes Python SDK (ideal-postcodes on PyPI). Covers postcode and UPRN lookup, address cleanse, email and phone validation, API keys, licensees and error handling, plus a reference for every API operation.
license: SEE LICENSE IN LICENSE
metadata:
  author: ideal-postcodes
  homepage: https://ideal-postcodes.co.uk
references:
  - setup.md
  - postcode-lookup.md
  - uprn-lookup.md
  - address-cleanse.md
  - email-validation.md
  - phone-validation.md
  - keys.md
  - licensees.md
  - errors.md
  - common.md
  - endpoints/
---

# Ideal Postcodes Python SDK

Call the Ideal Postcodes API from Python 3.12 or later with typed methods and Pydantic v2 response models. The client is synchronous and built on HTTPX.

```bash
pip install ideal-postcodes
```

```python
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    for address in client.postcodes("BR8 7RE"):
        print(address.line_1, address.post_town, address.uprn)
```

Create one client and reuse it; close it with `with` or `close()`. Path parameters are positional, everything else is keyword-only. Methods return the API's `result` as models: read attributes, or call `model_dump()` for a dictionary.

## Scope

Every API operation has a method, including autocomplete (`find_address`, `resolve_address`), place search, configs, licensees, email and phone validation. The guides cover the common tasks; `references/endpoints/` has one page per method.

## Authentication and errors

Pass the API key to the client. Key management (except `key_availability`), licensee and config methods also need a Management Key (`uk_...`) as the `user_token` keyword argument. The SDK sends it in the `Authorization` header; load it from a secret store and never ship it to a browser. Unsuccessful responses raise `APIError` subclasses (`AuthenticationError`, `PaymentRequiredError`, `NotFoundError`, `RateLimitError`, `ServerError`) with `status`, `code` and `message`. Network failures raise `TransportError`; invalid responses raise `ProtocolError`. All derive from `ClientError`. The SDK never retries: a timed-out lookup may already be charged. See [error handling](./references/errors.md).

## References

- [Setup](./references/setup.md)
- [Postcode lookup](./references/postcode-lookup.md)
- [UPRN lookup](./references/uprn-lookup.md)
- [Address cleanse](./references/address-cleanse.md)
- [Email validation](./references/email-validation.md)
- [Phone validation](./references/phone-validation.md)
- [API keys](./references/keys.md)
- [Licensees](./references/licensees.md)
- [Error handling](./references/errors.md)
- [Responses, arguments and testing](./references/common.md)
- [Operations](./references/endpoints/)
