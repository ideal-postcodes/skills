# Address Match

**Schema name:** `GbrCleanseMatch`

## Fields

| Field | Required | Type | Description | Example |
|---|---|---|---|---|
| `query` | yes | string | Originally submitted query |  |
| `match` | yes | [Address](./address.md) | Nearest matching address |  |
| `count` | yes | number | The number of addresses we matched to the input. We return the closest match by default. |  |
| `fit` | yes | number | A score represented as number between 1 and 0. Fit compares the address elements present in your query against the matching address elements. It does not incorporate elements you have not presented in the score. A partial address (e.g. 12 Pye Green Road) will have a fit of 1 even though it is missing post town and postcode. Its confidence score will be less than 1 however because it is missing some crucial elements. |  |
| `confidence` | yes | number | A confidence score represented as number between 1 and 0. 1 indicates a full match. 0 indicates no complete matching elements. |  |
| `organisation_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the organisation |  |
| `premise_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the premise |  |
| `postcode_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the postcode |  |
| `thoroughfare_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the street |  |
| `locality_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the locality |  |
| `post_town_match` | yes | `FULL` \| `PARTIAL` \| `INCORRECT` \| `MISSING` \| `NA` | Match indicator for the post_town |  |

## Used By

- [AddressCleanse](../endpoints/address-cleanse.md)
