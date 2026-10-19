# Email validation

`email_validation` checks whether an email address can receive mail before you store it. It also flags disposable, free, role and catch-all addresses.

```python title="email_validation.py"
from ideal_postcodes import IdealPostcodes

with IdealPostcodes("ak_test") as client:
    email = client.email_validation(query="support@ideal-postcodes.co.uk")
    print(email.result, email.deliverable, email.suggestions)
```

Your key needs email validation enabled.

## Result

`result` is one of three values:

| `result` | Meaning | `deliverable` |
| --- | --- | --- |
| `deliverable` | The mailbox accepts mail | `True` |
| `not_deliverable` | Mail will bounce, or the domain cannot receive mail | `False` |
| `unknown` | The API could not check the address | `None` |

For a `deliverable` or `not_deliverable` result, four more flags are `True` or `False`:

- `disposable`: a throwaway domain, such as temp-mail.org
- `free`: a free provider, such as gmail.com or outlook.com
- `role`: a role mailbox, such as admin@ or support@
- `catchall`: the domain accepts mail for any mailbox, so a `deliverable` result proves less

For `unknown`, every flag is `None`.

## Suggested corrections

Sometimes. When the domain does not resolve or publishes no MX records, `suggestions` can hold corrected email addresses. The API does not validate them, so check a suggestion before you use it. Every other result returns an empty list.

## Cost

One lookup for a checked address. An `unknown` result costs nothing, and neither does a domain that does not resolve or has no MX records. The API rejects an address over 320 characters with an `APIError`.

See [`email_validation`](https://docs.ideal-postcodes.co.uk/docs/sdks/python/endpoints/email-validation) for the parameters.
