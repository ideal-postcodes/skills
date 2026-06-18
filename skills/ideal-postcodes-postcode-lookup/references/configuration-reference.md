# Configuration Reference

A complete reference for every option accepted by [`PostcodeLookup.setup`](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/configure). Only [`context`](#context), [`apiKey`](#apikey) and [`outputFields`](#outputfields) are required; everything else is optional and has a sensible default.

Callbacks have their own page - see [Callbacks](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks). Message strings and CSS classes are summarised here and covered in depth under [Messages](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/messages) and [CSS Classes](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/css-classes).

### `context`

`string | HTMLElement` · required

The area on your page where Postcode Lookup renders its interface - typically a `<div>`, referenced by CSS selector or a direct element reference.

### `apiKey`

`string` · required

API Key from your Ideal Postcodes account. Typically begins `ak_`.

### `outputFields`

`object` · default `{}`

Specifies where to send address data when an address is selected. Maps an address attribute to an input field, identified by a CSS selector `string` or an `HTMLElement`.

```javascript
{
  line_1: "#line_1",
  line_2: "#line_2",
  line_3: "#line_3",
  post_town: document.getElementById("post_town"),
  postcode: document.getElementById("postcode")
}
```

The attribute names match the Address response object - e.g. street name populates from [`thoroughfare`](https://docs.ideal-postcodes.co.uk/docs/data/paf#thoroughfare). The full list is in the [UK address data guide](https://docs.ideal-postcodes.co.uk/docs/data/paf). Fields assigned with a query selector are evaluated lazily.

### `strictlyPostcodes`

`boolean` · default `true`

When `true`, only postcodes are searched. Set to `false` to search the term as a complete address, enabling queries such as `SW1A 2AA 10`.

### `limit`

`number` · default `10`

Maximum number of addresses presented after an address search. Only applies when `strictlyPostcodes` is `false`.

### `cooloff`

`number` · default `500`

Milliseconds the search button stays disabled after a click, preventing double presses. Set to `0` to disable.

### `selectSinglePremise`

`boolean` · default `false`

When `true`, a single-premise result is populated immediately without showing the selection dropdown. [`onAddressSelected`](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks) still fires.

### `autoSearchSingleSuggestion`

`boolean` · default `true`

When a postcode lookup fails but returns exactly one suggestion, the input is updated with that suggestion and a new search is triggered automatically.

### `titleizePostTown`

`boolean` · default `true`

Title-case the post town (`true`) or keep it in all caps (`false`). All caps is recommended by Royal Mail's good addressing guidelines.

### `removeOrganisation`

`boolean` · default `false`

Strips the organisation name from the address lines. Note that some "Large User" addresses consist only of an organisation name, post town and postcode.

### `populateCounty`

`boolean` · default `true`

Set to `false` to suppress `county` from being populated.

## Input field

| Option | Type | Default |
|---|---|---|
| `input` | `string \| HTMLInputElement \| null` | `null` - supply your own input instead of the generated one |
| `inputId` | `string \| null` | `null` |
| `inputClass` | `string \| null` | `"idpc-input"` |
| `inputAriaLabel` | `string` | `"Search a postcode to retrieve your address"` |
| `placeholder` | `string` | `"Search your postcode"` |
| `inputStyle` | `object` | `{}` - inline styles for the input |
| `autocomplete` | `string` | `"none"` - sets the input's `autocomplete=` attribute to prevent clashing browser autofill |

## Button

| Option | Type | Default |
|---|---|---|
| `button` | `string \| HTMLButtonElement \| null` | `null` - supply your own button |
| `buttonId` | `string \| null` | `null` |
| `buttonLabel` | `string` | `"Find my Address"` |
| `buttonAriaLabel` | `string` | `"Search for address using postcode"` |
| `buttonClass` | `string \| null` | `"idpc-button"` |
| `buttonStyle` | `object` | `{}` - inline styles for the button |

## Address dropdown

| Option | Type | Default |
|---|---|---|
| `selectContainer` | `string \| HTMLDivElement \| null` | `null` - element the results `<select>` is inserted into |
| `selectContainerId` | `string \| null` | `null` |
| `selectContainerClass` | `string \| null` | `"idpc-select-container"` |
| `selectId` | `string \| null` | `null` |
| `selectClass` | `string \| null` | `"idpc-select"` |
| `selectAriaLabel` | `string \| null` | `"Select your address"` |
| `selectStyle` | `object` | `{}` - inline styles for the `<select>` |

### `postcodeSearchFormatter`

`function`

Converts an address object into the suggestion string shown in the dropdown after a postcode search.

### `addressSearchFormatter`

`function`

Converts an address object into the suggestion string shown after a freeform address search.

## Messages

Override any user-facing string. See [Messages](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/messages).

| Option | Default |
|---|---|
| `msgSelect` | `"Please select your address"` |
| `msgDisabled` | `"Finding addresses..."` |
| `msgNotFound` | `"Your postcode could not be found. Please type in your address"` |
| `msgAddressNotFound` | `"We could not find a match for your address. Please type in your address"` |
| `msgError` | `"Sorry, we weren't able to get the address you were looking for. Please type in your address"` |
| `msgUnhide` | `"Enter address manually"` |

## Message container

| Option | Type | Default |
|---|---|---|
| `message` | `string \| HTMLParagraphElement \| null` | `null` - supply your own message container |
| `messageId` | `string \| null` | `null` |
| `messageClass` | `string \| null` | `"idpc-error"` |
| `messageStyle` | `object` | `{}` - inline styles for the message container |

### `hide`

`(string | HTMLElement)[]` · default `[]`

Elements to hide with `display: none;` when Postcode Lookup initialises. They are unhidden when an address is selected or the user opts for manual entry.

```javascript
{
  hide: [
    "#line_1",
    document.getElementById("line_2"),
    document.querySelector("#line_3"),
  ],
}
```

Enabling this also renders a clickable element offering manual entry. Provide your own with [`unhide`](#unhide), or customise the default with [`msgUnhide`](#messages) and [`unhideClass`](#unhideclass).

### `unhide`

`string | HTMLElement | null` · default `null`

A custom clickable element to unhide fields hidden with [`hide`](#hide).

### `unhideClass`

`string` · default `"idpc-unhide"`

CSS class applied to the default unhide element.

### `contextStyle`

`object` · default `{}`

Inline styles applied to the `context` container.

### `checkKey`

`boolean` · default `true`

Checks the key is usable before initialising. The check can fail if:

- Your key has no remaining lookups
- Your key has run into its daily lookup limit
- Your key is requested from a URL not on your allowed list
- The user has exceeded their daily limit

On failure the widget does not initialise. Use the [`onFailedCheck`](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks) callback to handle this case.

## DOM scoping

| Option | Type | Default |
|---|---|---|
| `scope` | `Document \| HTMLElement \| string` | `window.document` - scopes the operable area of the DOM |
| `document` | `Document` | `window.document` |
| `outputScope` | `string \| HTMLElement \| Document \| null` | `null` - narrows where output fields are searched for |

## Network and client

Inherited from the underlying API client. Defaults suit the production API and rarely need changing.

| Option | Type | Default |
|---|---|---|
| `baseUrl` | `string` | `"api.ideal-postcodes.co.uk"` |
| `version` | `string` | `"v1"` |
| `tls` | `boolean` | `true` |
| `timeout` | `number` | `10000` (ms) |
| `strictAuthorisation` | `boolean` | `false` |
| `header` | `object` | `{}` |
| `tags` | `string[]` | `[]` |

## Callbacks

Every lifecycle hook is documented on the [Callbacks](https://docs.ideal-postcodes.co.uk/docs/postcode-lookup/callbacks) page: `onLoaded`, `onFailedCheck`, `onButtonTrigger`, `onLookupTriggered`, `shouldLookupTrigger`, `onSearchCompleted`, `onSearchError`, `onAddressesRetrieved`, `onAddressSelected`, `onAddressPopulated`, `onSelectCreated`, `onSelectRemoved`, `onUnhide` and `onRemove`.
