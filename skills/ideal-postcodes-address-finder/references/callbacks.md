# Callbacks

Address Finder also provides callbacks which let you hook into the following events:

## Address Fields Populated

[`onAddressPopulated`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onAddressPopulated)

Invoked after the selected address is applied to input fields.

## Full Address Retrieved from API

[`onAddressRetrieved`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onAddressRetrieved)

Invoked when the Address Finder client has retrieved a full address from the API following a user accepting a suggestion. The first argument is an object representing the address that has been retrieved.

## User Selects an Address Suggestion

[`onAddressSelected`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onAddressSelected)

Invoked immediately after the user has selected a suggestion (either by click or keypress). The first argument is an object which represents the suggestion selected.

## Address Finder Field Loses Focus

[`onBlur`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onBlur)

Invoked when the user unfocuses from the address input field.

## Address Suggestion List Closes

[`onClose`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onClose)

Invoked when Address Finder suggestion box is closed (i.e. hidden from user).

## API Key Check Fails

[`onFailedCheck`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onFailedCheck)

Invoked if checkKey is enabled and the check fails.

## Address Field is Selected

[`onFocus`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onFocus)

Invoked when the user selects or focuses the address input field.

## User Inputs into Address Finder Field

[`onInput`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onInput)

Invoked when the user edits the Address Finder field.

## User Presses a Key

[`onKeyDown`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onKeyDown)

Invoked when a keypress is triggered on the input.

## Address Finder Successfully Loads

[`onLoaded`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onLoaded)

Invoked when Address Finder has been successfully attached to the input element.

## Address Finder is Mounted to DOM

[`onMounted`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onMounted)

Invoked when controller attaches to the DOM (controller.view.attach()).

## User Clicks on Suggestion List

[`onMouseDown`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onMouseDown)

Invoked when mousedown event is triggered on suggestion list.

## Address Suggestion List Opens

[`onOpen`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onOpen)

Invoked when Address Finder suggestion box is opened (i.e. presented to the user).

## Address Finder Detaches from DOM

[`onRemove`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onRemove)

Invoked when controller detaches from the DOM (controller.view.detach()).

## Full Address Retrieval Fails

[`onSearchError`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onSearchError)

Invoked when an error has occurred following an attempt to retrieve a full address. In this scenario the user will also receive a message to manually input their address.

The first argument is an error instance (i.e. inherits from Error) representing the error which has occurred.

Examples of errors include "lookup balance exhausted" and "lookup limit reached" errors.

## Address Suggestion is Selected

[`onSelect`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onSelect)

Invoked when a suggestion has been selected.

## Address Suggestion Retrieval Fails

[`onSuggestionError`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onSuggestionError)

Invoked when an error has occurred following an attempt to retrieve suggestions for a key press. In this scenario the user will also receive a message to manually input their address.

The first argument is an error instance (i.e. inherits from Error) representing the error which has occurred.

Examples of errors include "lookup balance exhausted" and "lookup limit reached" errors.

## Address Suggestions Retrieved from API

[`onSuggestionsRetrieved`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onSuggestionsRetrieved)

Invoked immediately after address suggestions are retrieved from the API. The first argument is an array of address suggestions.

## Hidden Address Fields are Unhidden

[`onUnhide`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onUnhide)

Invoked when hidden fields are unhidden (i.e. user selects an address or opts for manual input)
