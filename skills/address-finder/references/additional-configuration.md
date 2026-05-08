# Additional Configuration

## Separate Input Field

[`inputField`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#InputField)

Configure `inputField` if you want Address Finder to render on an input field which is not your first address line.

CSS selector or HTML Element which specifies the `<input>` field which the Address Finder interface should bind.

Defaults to `outputFields.line_1` (the first address line).

## Disable Key Check

[`checkKey`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#CheckKey)

Enabled by default. This causes Address Finder to check if the key is in a usable state before initialising itself.

Defaults to `true`.

The check can fail if:

- Your key has no remaining lookups
- Your key has run into its lookup limit for the day
- Your key is requested from a URL which is not on your whitelist
- The user seeking to use the key has exceeded their limit for the day

If the check fails, the plugin will not initialise. You can use the [`onFailedCheck`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#onFailedCheck) callback to customise your response to a failed check.

## Uppercase Post Town

[`titleizePostTown`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#TitleizePostTown)

An optional field to convert the case of the Post Town from upper case into title case. E.g. `"LONDON"` becomes `"London".`

Defaults to `true`.

## Query Options

[`queryOptions`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#QueryOptions)

Apply a query, like a filter or bias, to your suggestion queries. A complete list of query parameters are available in the [Autocomplete API Reference](https://docs.ideal-postcodes.co.uk/docs/api/find-address).

To apply new parameters after the Controller has been instantiated, use [`setQueryOptions`](https://address-finder.ideal-postcodes.co.uk/classes/Controller.Controller-1.html#SetQueryOptions). Do not mutate `options.queryOptions` directly.

Defaults to `{}`.

## Resolve Options

[`resolveOptions`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#ResolveOptions)

Apply additional query parameters to the address resolve request. This is the second API call that retrieves the full address when a user selects a suggestion. A complete list of query parameters are available in the [Resolve Address API Reference](https://docs.ideal-postcodes.co.uk/docs/api/resolve-address).

To apply new parameters after the Controller has been instantiated, use [`setResolveOptions`](https://address-finder.ideal-postcodes.co.uk/classes/Controller.Controller.html#setResolveOptions). Do not mutate `options.resolveOptions` directly.

Defaults to `{}`.

## Remove Organisation from Address Line

[`removeOrganisation`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#RemoveOrganisation)

If set to `true`, organisation name will be removed from the address.

Defaults to `false`.

## Disable Chrome Autofill

[`autocomplete`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#Autocomplete)

Sets the `autocomplete=` attribute of the input element. Setting this attribute aims to prevent some browsers (particularly Chrome) from providing a clashing autofill overlay.

The best practice for this attribute breaks over time (see [https://stackoverflow.com/questions/15738259/disabling-chrome-autofill](https://stackoverflow.com/questions/15738259/disabling-chrome-autofill)) and is specific to different forms. If you are observing Chrome's autofill clashing on your form, update this attribute to the best practice du jour.

Defaults to `"none"`.

## Hide Address Fields

[`hide`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#Hide)

Accepts an array of HTMLElements or CSS selectors. E.g.

```javascript
{
  hide: [
    "#line_1",
    document.getElementById("line_2"),
    document.getElementById("line_3")],
  ],
}
```

When enabled, the HTMLElements supplied in `hide` are hidden with `display: none;` when Address Finder initialises and attaches to the DOM. These elements will be subsequently unhidden if an address is selected or the user opts to manually input an address.

Enabling this feature will also create a clickable element in the Address Finder, which provides users the option to manually input an address. You may provide your own clickable element with `unhide` or customise the clickable element with `msgUnhide` and `unhideClass`.

Defaults to `[]`.

## Custom Unhide Element

[`unHide`](https://address-finder.ideal-postcodes.co.uk/interfaces/Controller.ControllerOptions.html#Unhide)

Specify a query selector (`string`) or (`HTMLElement`) as a custom clickable element to unhide and form fields configured with `hide`. This will prevent the default unhide element from being rendered.
