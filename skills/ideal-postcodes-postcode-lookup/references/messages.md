# Messages

Postcode Lookup uses a small default set of messages to communicate specific events to the user.  All of Postcode Lookup's default messages can be overridden. They are outlined below:

## Postcode Lookup Placeholder

`placeholder`

Sets the default placeholder label on input field.

## Button Label

`buttonLabel`

Sets the label on the search button.

## Postcode Lookup Disabled

`msgDisabled`

Message shown when button is temporarily disabled due to lookup.

## An Error Occurred

`msgError`

Message shown when an error occurs and prevents address verification from succeeding.

## Postcode Not Found

`msgNotFound`

Message shown when an invalid or terminated postcode is searched.

## Address Dropdown Prompt

`msgSelect`

Message shown at the top of the address `<select>` dropdown.

## Unhide Address Fields Message

`msgUnhide`

Message shown to unhide address fields and allow manual address entry.

## WAI ARIA

Postcode Lookup exposes some accessibility settings.

- Search input aria label `inputAriaLabel`
- Address select aria label `selectAriaLabel`

The following WAI-ARIA components of Postcode Lookups have WAI-ARIA attributes enabled.

- Message box `role` is set to `alert` to immediately notify the user
- Postcode Lookup container has `aria-live` enabled to alert users to updates in the address dropdown
- Search input accepts an Enter/Return key to trigger an address search without submitting the form

> **Accessibility**
> 
> Bear in mind these messages are also important for users who rely on the screen reader. When altering the message, be sure that the necessary meaning and intent are understandable to visually impaired users who will be getting audio cues from their screen reader.
