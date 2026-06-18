# Callbacks

Postcode Lookup also provides callbacks which let you hook into specific events.

## Address Applied to Fields

`onAddressPopulated`

A function invoked after a selected address has been applied to output fields.

## User Selects an Address from Dropdown

`onAddressSelected`

A function invoked when the user selects an address option on the plugin's dropdown menu. This property takes a function, which accepts an address argument representing the full details of the selected address.

## Addresses Retrieved from API

`onAddressesRetrieved`

A function invoked when a successful API request is made and at least 1 address is returned. This property takes a function, which accepts an `addresses` argument representing the addresses returned by the API.

## User Clicks Search Button

`onButtonTrigger`

A function invoked when the search button is triggered (i.e. clicked, pressed).

## User Triggers a Lookup

`onLookupTriggered`

A function invoked when the user triggers a lookup (e.g. by clicking the button). This callback is invoked before any request is made to the API.

## Postcode Lookup is Removed from DOM

`onRemove`

Invoked when `removeAll` is called.

## API Request Completed

`onSearchCompleted`

A function invoked as soon as an address search successfully completes.

Note, this is also invoked on all errors including:
- 404-type errors such as "postcode not found" returning [] as the list of addresses.
- Other 4XX and 5XX errors where the error argument will be available

## API Request Failed

`onSearchError`

A function invoked when the API returns an error. This property takes a function, which accepts an error argument representing the error returned by the API.

Examples of errors include "lookup balance exhausted" and "lookup limit reached" errors.

## Address Dropdown is Created

`onSelectCreated`

A function invoked when addresses have been retrieved and the dropdown has been inserted into the DOM.

## Address Dropdown is Removed

`onSelectRemoved`

A function invoked when select element is removed from the DOM as user attempts to search another address or postcode.

## Hidden Address Fields are Unhidden

`onUnhide`

A function invoked when fields defined in hide are unhidden. Useful for further customisation when address fields are finally presented to the user.

This function must be idempotent as it can be invoked multiple times during both hidden and unhidden states.

## Should Lookup Trigger

`shouldLookupTrigger`

A function invoked just before the plugin triggers a lookup when the search button is clicked. If false is returned, the search API request is aborted.

## API Key Check Fails

`onFailedCheck`

A function invoked if checkKey is enabled and the API Key check fails.
