# Error Codes and Common Fixes

Common API error codes with causes and fixes, ordered by how frequently we observe the resolutions.

## 4010 - Invalid Key {#4010}

**Message:** `Invalid Key`
**HTTP Status:** 401

Your API Key was not recognised. The key may be incorrect or malformed.

### Potential Fixes

1. **Check for typos** — copy the key directly from your dashboard.
2. **Check querystring parameter name** — ensure the key is passed as `api_key` and not `api-key`.
3. **Check Authorization header format** — ensure the header is formatted as `IDEALPOSTCODES api_key="ak_yourkey"`.

## 4011 - URL Not on Allowed List {#4011}

**Message:** `Requesting URL not on whitelist`
**HTTP Status:** 401

The request's `Referer` or `Origin` header did not match any URL on your key's [allowed URL list](https://docs.ideal-postcodes.co.uk/docs/guides/allowed-urls).

### Potential Fixes

1. **Check if you need Allowed URLs** — non-browser requests won't contain the `Referer` or `Origin` headers needed for matching. Remove Allowed URLs if the key is kept private.
2. **Review your Allowed URL configuration** — check the [API Key security guide](https://docs.ideal-postcodes.co.uk/docs/guides/api-key-secure) to ensure URLs have been defined correctly.

## 4020 - Balance Depleted {#4020}

**Message:** `Key balance depleted`
**HTTP Status:** 402

Your API Key has no remaining lookup balance.

### Potential Fixes

1. **Top up your balance** — purchase more lookups from your dashboard.
2. **Enable automated top-ups** — prevent this from recurring by [enabling automated top-ups](https://docs.ideal-postcodes.co.uk/docs/guides/automated-topups).

## 4021 - Lookup Limit Reached {#4021}

**Message:** `Lookup Limit Reached`
**HTTP Status:** 402

Your API Key has a daily or IP rate limit configured, and it has been reached.

### Potential Fixes

1. **Disable the rate limit** — remove the responsible limit in your key settings for an immediate fix.
2. **Increase the limit** — adjust the daily or IP limit in your key settings.
