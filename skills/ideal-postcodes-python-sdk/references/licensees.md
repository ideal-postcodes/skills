# Licensees

A licensee is a client of yours that uses Ideal Postcodes data under your key. Sublicensing lets a platform serve many clients from one account and one bill, with each client's usage kept separate. The licensee methods create, list, update and cancel them.

Your key must be a sublicensing key; [contact us](https://ideal-postcodes.co.uk/contact-us) to convert one. Every method needs a [Management Key](https://docs.ideal-postcodes.co.uk/docs/sdks/python/setup#management-key). See the [sublicensing guide](https://docs.ideal-postcodes.co.uk/docs/guides/sublicensing) for how licensing and billing work.

## Create a licensee

Call `create_licensee` with the client's legal name and address. The result carries the licensee's own key, beginning `sl_`, in `key`.

```python title="create_licensee.py"
import os

from ideal_postcodes import IdealPostcodes

with IdealPostcodes(os.environ["IDEAL_POSTCODES_API_KEY"]) as client:
    licensee = client.create_licensee(
        "ak_test",
        user_token=os.environ["IDEAL_POSTCODES_MANAGEMENT_KEY"],
        name="Example Ltd",
        address="9 Malyons Road, Swanley",
        postcode="BR8 7RE",
    )
    print(licensee.key)
```

Your licensees must accept Royal Mail's terms of use. Including them in your own terms is enough.

## List licensees

`list_licensees` returns 10 per call by default and up to 100 with `limit`, oldest first, without cancelled ones. `query` filters by name. To fetch the next page, pass the last licensee's `id` as `starting_after`.

## Update or cancel a licensee

Pass the key and the licensee's `sl_` key:

- `update_licensee` changes the address, postcode, allowed URLs (`whitelist`) or daily limit. The name is fixed once created
- `regenerate_licensee_key` issues a new `sl_` key. The old one stops working at once
- `delete_licensee` cancels the licensee. Its key stops working. Contact us to reverse it

## Billing

Each licensee buys lookups in blocks of 100, so a client who uses 60 lookups is billed for 100 and the next 40 cost nothing. See the [sublicensing guide](https://docs.ideal-postcodes.co.uk/docs/guides/sublicensing#billing). Report one licensee's usage by passing `licensee` to `key_usage` or `key_logs`.
