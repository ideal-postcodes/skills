# No Address Match

**Schema name:** `GbrCleanseNoMatch`

## Fields

| Field | Required | Type | Description | Example |
|---|---|---|---|---|
| `query` | yes | string | Originally submitted query |  |
| `match` | yes | `null` | Nearest matching address |  |
| `count` | yes | `0` |  |  |
| `fit` | yes | `0` |  |  |
| `confidence` | yes | `0` |  |  |
| `organisation_match` | yes | `NO_MATCH` |  |  |
| `premise_match` | yes | `NO_MATCH` |  |  |
| `postcode_match` | yes | `NO_MATCH` |  |  |
| `thoroughfare_match` | yes | `NO_MATCH` |  |  |
| `locality_match` | yes | `NO_MATCH` |  |  |
| `post_town_match` | yes | `NO_MATCH` |  |  |

## Used By

- [AddressCleanse](../endpoints/address-cleanse.md)
