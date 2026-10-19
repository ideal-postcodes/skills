# API keys

The key methods report whether a key can take requests, read or change its limits and export its usage. All of them except `key_availability` need a [Management Key](https://docs.ideal-postcodes.co.uk/docs/sdks/python/setup#management-key).

## Key availability

Call `key_availability`. It needs only the API key and costs nothing, so it suits a health check.

```python title="availability.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    key = client.key_availability("ak_test")
    print(key.available, [country.iso_3 for country in key.contexts])
```

`available` is `False` when the key has no lookups left, has hit a daily limit or is called from a URL it does not allow. `contexts` lists the countries the key can search.

## Balance and limits

`key_details` returns `lookups_remaining`, the daily, monthly and per-IP limits, allowed URLs, notification settings and datasets.

```python title="details.py"
import os

from ideal_postcodes import IdealPostcodes

with IdealPostcodes(os.environ["IDEAL_POSTCODES_API_KEY"]) as client:
    details = client.key_details("ak_test", user_token=os.environ["IDEAL_POSTCODES_MANAGEMENT_KEY"])
    print(details.lookups_remaining)
```

## Update limits

Call `update_key_details` with only the settings to change. The rest stay as they are.

```python title="update.py"
import os

from ideal_postcodes import IdealPostcodes

with IdealPostcodes(os.environ["IDEAL_POSTCODES_API_KEY"]) as client:
    client.update_key_details(
        "ak_test",
        user_token=os.environ["IDEAL_POSTCODES_MANAGEMENT_KEY"],
        daily_limit={"limit": 1000},
        allowed_urls=["https://www.example.com"],
    )
```

Nested settings are typed dictionaries from `ideal_postcodes.requests`, and `None` inside them becomes JSON `null`. So `daily_limit={"limit": None}` removes the daily limit, `allowed_urls=[]` removes the URL restriction and `notifications={"enabled": False}` stops notification emails.

## Usage and logs

`key_usage` returns totals and a daily breakdown. `key_logs` returns each billable lookup as CSV text. Both take `start` and `end` as UNIX timestamps in milliseconds.

```python title="usage.py"
import os
import time

from ideal_postcodes import IdealPostcodes

end = int(time.time() * 1000)
start = end - 7 * 24 * 60 * 60 * 1000

with IdealPostcodes(os.environ["IDEAL_POSTCODES_API_KEY"]) as client:
    token = os.environ["IDEAL_POSTCODES_MANAGEMENT_KEY"]
    usage = client.key_usage("ak_test", user_token=token, start=start, end=end)
    csv = client.key_logs("ak_test", user_token=token, start=start, end=end)
```

Pass `licensee` to either method to report one [licensee's](https://docs.ideal-postcodes.co.uk/docs/sdks/python/licensees) usage.
