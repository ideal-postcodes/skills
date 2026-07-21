# Messages

Address Finder uses a small default set of messages to communicate specific events to the user. All of Address Finder's default messages can be overridden. They are outlined below:

## Initial Message

`msgInitial`

Displays when the Address Finder is opened and the input field is empty.

Defaults to `"Start typing to find address"`

## Accessibility (Screen Reader) Message

`msgList`

The aria-label assigned to the address suggestion list. This message will only be picked up by screen readers.

Defaults to `"Select your address"`

## No Match

`msgNoMatch`

Displays when no suggestions could be found for the query.

Defaults to `"No matches found"`

## No Match Action

`msgNoMatchAction`

Label for an optional actionable item presented beneath `msgNoMatch` when a search returns no matches. Use it to offer users an out when they cannot find their address, e.g. a jump to manual address entry. The item renders when both a non-empty label and an `onNoMatchAction` callback are provided; selecting it by click or keyboard closes Address Finder and invokes the callback.

Defaults to `""` (disabled)

```javascript
AddressFinder.setup({
  apiKey: "ak_test",
  inputField: "#line_1",
  msgNoMatchAction: "Enter address manually",
  onNoMatchAction: function () {
    // e.g. reveal a manual address entry form
  },
});
```

## Unhide Label

`msgUnhide`

Displayed to the user if address fields have been hidden. It is shown as a clickable element to trigger manual address entry.

Defaults to `"Enter address manually"`

## Fallback Message

`msgFallback`

Shown when Address Finder is unable to provide suggestions for any reason (e.g. key is not available).

Defaults to `"Please enter your address manually"`

> **Accessibility**
> 
> Bear in mind these messages are also important for users who rely on a screen reader. When altering the message, be sure that the necessary meaning and intent are understandable to visually impaired users who will be getting audio cues from their screen reader.
