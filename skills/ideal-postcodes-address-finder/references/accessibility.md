# Accessibility

Address Finder is developed to meet [WCAG 2.1 Level AA](https://www.w3.org/TR/WCAG21/). It implements the WAI-ARIA combobox pattern, works with screen readers and is operable by keyboard.

## Screen Reader Support

The suggestion list is exposed as an ARIA combobox: the input carries `role="combobox"`, `aria-expanded` and `aria-controls`, and the suggestion list is a `listbox` of `option` elements. DOM focus stays on the input at all times - the highlighted suggestion is conveyed via `aria-activedescendant`, so screen reader users hear each suggestion as they arrow through it, along with its position in the list.

By default Address Finder uses ARIA 1.0 authoring, which has the widest screen reader support (notably VoiceOver and NVDA). The [`aria` option](https://docs.ideal-postcodes.co.uk/docs/address-finder/configuration-reference) switches to ARIA 1.1 authoring if preferred.

The `aria-label` on the suggestion list can be customised with the `msgList` option - see [Messages](https://docs.ideal-postcodes.co.uk/docs/address-finder/messages).

## Keyboard Support

| Key | Action |
| --- | --- |
| <kbd>↓</kbd> / <kbd>↑</kbd> | Move through suggestions, wrapping at either end |
| <kbd>Enter</kbd> | Select the highlighted suggestion and populate the form |
| <kbd>Escape</kbd> | Close the suggestion list and clear the input |
| <kbd>Home</kbd> / <kbd>End</kbd> | Return to the input |
| <kbd>Tab</kbd> | Move to the country toggle; press again to leave the widget |
| <kbd>Enter</kbd> / <kbd>Space</kbd> on the country toggle | Open the country list |

Clickable elements rendered by Address Finder (the country toggle, the [no-match action](https://docs.ideal-postcodes.co.uk/docs/address-finder/no-match-action), the [unhide link](https://docs.ideal-postcodes.co.uk/docs/address-finder/hide)) are focusable buttons and respond to <kbd>Enter</kbd> and <kbd>Space</kbd>.

## Testing

Every release is gated by automated WCAG A/AA scans and keyboard regression tests covering the interactions above.

If you find an accessibility issue, please [report it](https://github.com/ideal-postcodes/feedback/issues).
