# Address (list endpoints)

A single address, as returned by the postcode and address list endpoints. Every other endpoint returns `Address`, the same set of fields with `native` on every record.

The standard Ideal Postcodes address. Its fields follow the layout UK address databases typically use, and much of it reflects Royal Mail's Postcode Address File, the UK's primary address database.

The API converts non-UK addresses into the same UK layout so they insert into a standard address database.

Pay attention to the address lines (`line_1`, `line_2` and `line_3`), post town, postcode, county and country. Together they are all you need to identify an address uniquely, in the UK or as an international address.

For international addresses, cities map to `post_town` and states map to `county`.

Addresses from AddressBase (`ab`, `abp`) and non-UK datasets also carry a `native` object: the raw record from its source dataset, with local detail the standard fields cannot hold. It is never returned for the Royal Mail PAF family (`paf`, `mr`, `nyb`, `pafa`, `pafw`) on these two endpoints. E.g.

- ECAD records say whether an address sits in a Gaeltacht (Irish-speaking) district and whether the building is residential or commercial
- USPS records carry the carrier route and congressional district
- Kadaster records carry the floor area, year of completion and use (residential, office, retail)

**Schema name:** `AddressListItem`

## Fields

| Field | Required | Type | Description | Example |
|---|---|---|---|---|
| `id` | yes | string | Global unique internally generated identifier for an address |  |
| `dataset` | yes | string | Indicates the provenance of an address. |  |
| `country_iso` | yes | string | 3 letter country code (ISO 3166-1) |  |
| `country_iso_2` | yes | string | 2 letter country code (ISO 3166-1) |  |
| `country` | yes | string | Full country names (ISO 3166) |  |
| `language` | yes | string | Language represented by 2 letter ISO Code (639-1) |  |
| `line_1` | yes | string | First address line. Often contains premise and thoroughfare information. For a commercial premise the first line is the full name of the registered organisation. Never empty. |  |
| `line_2` | yes | string | Second address line. Often contains thoroughfare and locality information. May be empty. |  |
| `line_3` | yes | string | Third address line. Takes the address elements left after `line_1` and `line_2` are filled; where the address needs more than three lines the remaining elements are joined into `line_3`, comma separated. May be empty. |  |
| `post_town` | yes | string | The town or city used to route mail to the address. For UK addresses this is the Royal Mail post town, which is a routing instruction rather than the nearest town geographically. Present on every address. |  |
| `postcode` | yes | string | Correctly formatted postcode. Capitalised and spaced. Empty (`""`) where the address has no postcode. |  |
| `county` | yes | string | Whatever county data is available for the address. Normally the postal county. If that is not present it falls back to the administrative county, then to the traditional county. May be empty where none of the three is present. |  |
| `county_code` | yes | string | Short code representing the county or province. May be empty (`""`) |  |
| `uprn` | yes | string | UPRN stands for Unique Property Reference Number and is maintained by the Ordnance Survey (OS). Local governments in the UK have allocated a unique number for each land or property. |  |
| `udprn` | yes | integer \| `""` | UDPRN stands for 'Unique Delivery Point Reference Number'. Royal Mail assigns a unique UDPRN code for each premise on PAF. Simple, unique reference number for each Delivery Point. Unlikely to be reused when an address expires. |  |
| `umprn` | yes | string \| number | A small minority of individual premises (as identified by a UDPRN) may have multiple occupants behind the same letterbox. These are known as Multiple Residence occupants and can be queried via the Multiple Residence dataset. Simple, unique reference number for each Multiple Residence occupant. |  |
| `postcode_outward` | yes | string | The first part of a postcode is known as the outward code. e.g. The outward code of ID1 1QD is ID1. Enables mail to be sorted to the correct local area for delivery. This part of the code contains the area and the district to which the mail is to be delivered, e.g. 'PO1', 'SW1A' or 'B23'. |  |
| `postcode_inward` | yes | string | The second part of a postcode is known as the inward code. e.g. The inward code of ID1 1QD is 1QD. |  |
| `dependant_locality` | yes | string | A locality that qualifies the thoroughfare. Used where the same thoroughfare name occurs more than once in a post town and no dependant thoroughfare distinguishes them. May be empty. |  |
| `double_dependant_locality` | yes | string | Supplements dependant locality. Supplied where the dependant locality itself occurs twice in the same locality. May be empty. |  |
| `thoroughfare` | yes | string | Also known as the street or road name. May be empty. |  |
| `dependant_thoroughfare` | yes | string | Supplements thoroughfare. Used where a thoroughfare name occurs twice in the same post town, to identify the address uniquely. May be empty. |  |
| `building_number` | yes | string | Number identifying the premise on a thoroughfare or dependant thoroughfare. May be empty. |  |
| `building_name` | yes | string | Name of a residential or commercial premise. May be empty. |  |
| `sub_building_name` | yes | string | Identifies a unit where a premise is split into flats, apartments or business units. Cannot be present without either building_name or building_number. E.g. Flat 1, A, 10B. May be empty. |  |
| `po_box` | yes | string | PO Box number for the address, occasionally a combination of numbers and letters. Allocated to Large User postcodes only. May be empty. |  |
| `department_name` | yes | string | Supplements organisation name to identify a department within the organisation. May be empty. |  |
| `organisation_name` | yes | string | Name of the business or organisation at this address. May be empty. |  |
| `postcode_type` | yes | `S` \| `L` \| `""` | Royal Mail postcode user type. UK addresses only. |  |
| `su_organisation_indicator` | yes | string | `Y` where an organisation is present at a small user postcode. Empty (`""`) otherwise. UK addresses only. |  |
| `delivery_point_suffix` | yes | string | Two-character code (the first numeric, the second alphabetical) which, added to the postcode, uniquely identifies a delivery point. May be reused once a delivery point is deleted, though not until every remaining code in the range has been allocated. Always `1A` for a large user postcode, since each large user has its own postcode. Empty (`""`) where not available. |  |
| `premise` | yes | string | A pre-computed string which sensibly combines building_number, building_name and sub_building_name. Those three fields hold raw dataset values and can be difficult to parse if you are unaware of how they work together, so we also provide this single, simple premise string. Ideal if you want to pull premise information and thoroughfare information separately instead of using our address lines data. |  |
| `administrative_county` | yes | string | The current administrative county to which the postcode has been assigned. |  |
| `postal_county` | yes | string | Postal counties were used for the distribution of mail before the Postcode system was introduced in the 1970s. The Former Postal County was the Administrative County at the time. This data rarely changes. May be empty. |  |
| `traditional_county` | yes | string | Traditional counties are provided by the Association of British Counties. It is historical data, and can date from the 1800s. May be empty. |  |
| `district` | yes | string | The current district/unitary authority to which the postcode has been assigned. May be empty. |  |
| `ward` | yes | string | The current administrative/electoral area to which the postcode has been assigned. May be empty for a small number of addresses. |  |
| `longitude` | yes | string \| number | The longitude of the address or postcode (WGS84). |  |
| `latitude` | yes | string \| number | The latitude of the address or postcode (WGS84). |  |
| `eastings` | yes | string \| number | Eastings reference using the [Ordnance Survey National Grid reference system](https://en.wikipedia.org/wiki/Ordnance_Survey_National_Grid). |  |
| `northings` | yes | string \| number | Northings reference using the [Ordnance Survey National Grid reference system](https://en.wikipedia.org/wiki/Ordnance_Survey_National_Grid) |  |
| `native` | no | [AbAddress](./ab-address.md) \| [AbpAddress](./abp-address.md) \| [UspsAddress](./usps-address.md) \| [EcadAddress](./ecad-address.md) \| [EcafAddress](./ecaf-address.md) \| [HereAddress](./here-address.md) \| [GnafAddress](./gnaf-address.md) \| [KadasterAddress](./kadaster-address.md) \| [KartverketAddress](./kartverket-address.md) \| [SdfiAddress](./sdfi-address.md) \| [CannarAddress](./cannar-address.md) \| [FodbosaAddress](./fodbosa-address.md) \| [MoisAddress](./mois-address.md) \| [UpujpAddress](./upujp-address.md) \| [BevAddress](./bev-address.md) \| [BanAddress](./ban-address.md) \| [SwtAddress](./swt-address.md) | The raw dataset record backing this address. On these two endpoints it is returned for AddressBase (`ab`, `abp`) and non-UK datasets only, never for the PAF family. Use any other endpoint for a PAF native record. |  |

## Example

```json
{
  "id": "paf_23747771",
  "dataset": "paf",
  "country_iso": "GBR",
  "country_iso_2": "GB",
  "country": "England",
  "language": "en",
  "postcode": "SW1A 2AA",
  "postcode_inward": "2AA",
  "postcode_outward": "SW1A",
  "post_town": "London",
  "dependant_locality": "",
  "double_dependant_locality": "",
  "thoroughfare": "Downing Street",
  "dependant_thoroughfare": "",
  "building_number": "10",
  "building_name": "",
  "sub_building_name": "",
  "po_box": "",
  "department_name": "",
  "organisation_name": "Prime Minister & First Lord Of The Treasury",
  "udprn": 23747771,
  "umprn": "",
  "uprn": "100023336956",
  "postcode_type": "S",
  "su_organisation_indicator": "",
  "delivery_point_suffix": "1A",
  "line_1": "Prime Minister & First Lord Of The Treasury",
  "line_2": "10 Downing Street",
  "line_3": "",
  "premise": "10",
  "longitude": -0.12767,
  "latitude": 51.503541,
  "eastings": 530047,
  "northings": 179951,
  "county": "London",
  "county_code": "",
  "traditional_county": "Greater London",
  "administrative_county": "",
  "postal_county": "London",
  "district": "Westminster",
  "ward": "St. James's"
}
```

## Used By

- [Postcodes](../endpoints/postcodes.md)
