# Callbacks

Address Finder also provides callbacks which let you hook into the following events:

## Address Fields Populated

`onAddressPopulated`

Invoked after the selected address is applied to input fields.

## Full Address Retrieved from API

`onAddressRetrieved`

Invoked when the Address Finder client has retrieved a full address from the API following a user accepting a suggestion. The first argument is an object representing the address that has been retrieved.

## User Selects an Address Suggestion

`onAddressSelected`

Invoked immediately after the user has selected a suggestion (either by click or keypress). The first argument is an object which represents the suggestion selected.

## Address Finder Field Loses Focus

`onBlur`

Invoked when the user unfocuses from the address input field.

## Address Suggestion List Closes

`onClose`

Invoked when Address Finder suggestion box is closed (i.e. hidden from user).

## API Key Check Fails

`onFailedCheck`

Invoked if checkKey is enabled and the check fails.

## Address Field is Selected

`onFocus`

Invoked when the user selects or focuses the address input field.

## User Inputs into Address Finder Field

`onInput`

Invoked when the user edits the Address Finder field.

## User Presses a Key

`onKeyDown`

Invoked when a keypress is triggered on the input.

## Address Finder Successfully Loads

`onLoaded`

Invoked when Address Finder has been successfully attached to the input element.

## Address Finder is Mounted to DOM

`onMounted`

Invoked when controller attaches to the DOM (controller.view.attach()).

## User Clicks on Suggestion List

`onMouseDown`

Invoked when mousedown event is triggered on suggestion list.

## Address Suggestion List Opens

`onOpen`

Invoked when Address Finder suggestion box is opened (i.e. presented to the user).

## Address Finder Detaches from DOM

`onRemove`

Invoked when controller detaches from the DOM (controller.view.detach()).

## Full Address Retrieval Fails

`onSearchError`

Invoked when an error has occurred following an attempt to retrieve a full address. In this scenario the user will also receive a message to manually input their address.

The first argument is an error instance (i.e. inherits from Error) representing the error which has occurred.

Examples of errors include "lookup balance exhausted" and "lookup limit reached" errors.

## Address Suggestion is Selected

`onSelect`

Invoked when a suggestion has been selected.

## Address Suggestion Retrieval Fails

`onSuggestionError`

Invoked when an error has occurred following an attempt to retrieve suggestions for a key press. In this scenario the user will also receive a message to manually input their address.

The first argument is an error instance (i.e. inherits from Error) representing the error which has occurred.

Examples of errors include "lookup balance exhausted" and "lookup limit reached" errors.

## Address Suggestions Retrieved from API

`onSuggestionsRetrieved`

Invoked immediately after address suggestions are retrieved from the API. The first argument is an array of address suggestions.

## Hidden Address Fields are Unhidden

`onUnhide`

Invoked when hidden fields are unhidden (i.e. user selects an address or opts for manual input).
