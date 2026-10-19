# Setup

Install `ideal-postcodes` from PyPI, create one `IdealPostcodes` client with your API key and reuse it for every request.

```bash
pip install ideal-postcodes
```

```python title="client.py"
import os

from ideal_postcodes import IdealPostcodes

client = IdealPostcodes(os.environ["IDEAL_POSTCODES_API_KEY"])
try:
    addresses = client.postcodes("BR8 7RE")
finally:
    client.close()
```

The client keeps a pool of HTTP connections open. Close it when your process finishes, or use it as a context manager with `with`.

## Client options

| Argument | Default | Purpose |
| --- | --- | --- |
| `api_key` | required | Your API key, beginning `ak_` |
| `timeout` | `10.0` | Seconds to wait for each request |
| `base_url` | `https://api.ideal-postcodes.co.uk/v1` | API root. Change it only to point at a test server |
| `transport` | `None` | An HTTPX transport, such as `httpx.MockTransport` in tests |

Every argument after `api_key` is keyword-only.

## Management Key

To read or change your account: key details, usage, logs, licensees and configs. A Management Key begins `uk_` and is on your [account page](https://account.ideal-postcodes.co.uk/account). Pass it to each of those methods as `user_token`. The SDK sends it in the `Authorization` header, never in the URL.

A Management Key controls your account. Load it from a secret store and never ship it to a browser or a mobile app.

## Async code

The client is synchronous. From an async application, run calls in a worker thread with `asyncio.to_thread`.

## User-Agent

Every request carries `User-Agent: ideal-postcodes-python-sdk/<version>`, such as `ideal-postcodes-python-sdk/0.1.0`. The header holds no credentials or address data.
