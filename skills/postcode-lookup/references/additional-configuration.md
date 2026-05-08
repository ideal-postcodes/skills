# Additional Configuration

## Postcode Search Only

[`strictlyPostcodes`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#strictlyPostcodes)

If enabled, any lookups which do not validate as a standard postcode will be passed to the address search API instead.

Defaults to `true`.

## Uppercase Post Town

[`titleizePostTown`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#titleizePostTown)

Title case the post town (`true`) or keep in all caps (`false`). All caps post town is recommended by Royal Mail's good addressing guidelines.

If enabled, any lookups which do not validate as a standard postcode will be passed to the address search API instead.

Defaults to `true`.

## Remove Organisation

[`removeOrganisation`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#removeOrganisation)

If set to `true`, organisation name will be removed from the address.

Note that addresses which are exclusively an organisation name will not result in the organisation name being removed as this will result in no premise identifier.

Defaults to `false`.

## Autoselect Single Premise Results

[`selectSinglePremise`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#selectSinglePremise)

If set to `true`, the premise will be immediately populated if the result set of an address or postcode search contains a single premise.

Note the address selection box will not appear.

Note that `onAddressSelected` callback is still invoked in this instance. To detect whether the last address search yielded a single premise, the controller instance data property will have a single element. i.e. `this.data.length === 1`.

Defaults to `false`.

## Hide Address Fields

[`hide`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#hide)

Accepts an array of HTMLElements or CSS selectors. E.g.

```javascript
{
  hide: [
    "#line_1",
    document.getElementById("line_2"),
    document.querySelector("#line_3")],
  ],
}
```

When enabled, the HTMLElements supplied in `hide` are hidden with `display: none;` when Postcode Lookup successfully initialises. These elements will be subsequently unhidden if an address is selected or the user opts to manually input an address.

Enabling this feature will also create a clickable element in the Postcode Lookup container, which provides users the option to manually input an address. You may provide your own clickable element with `unhide` or customise the clickable element with `msgUnhide` and `unhideClass`.

Defaults to `[]`.

## Prevent Chrome Autofill

[`autocomplete`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#autocomplete)

Sets the `autocomplete=` attribute of the input element. Setting this attribute aims to prevent some browsers (particularly Chrome) from providing a clashing autofill overlay.

The best practice for this attribute breaks over time (see https://stackoverflow.com/questions/15738259/disabling-chrome-autofill) and is specific to different forms. If you are observing chrome's autofill clashing on your form, update this attribute to the best practice du jour.

Defaults to `"none"`.

## Disable Key Checking 

[`checkKey`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#checkKey)

Set to `false` to disable key checking.

The check can fail if:
- Your key has no remaining lookups
- Your key has run into its lookup limit for the day
- Your key is requested from a URL which is not on your whitelist
- The user seeking to use the key has exceeded their limit for the day

If the check fails, the plugin will not initialise. You can use the [`onFailedCheck`](https://postcode-lookup.ideal-postcodes.co.uk/interfaces/Controller.ControllerConfig.html#onFailedCheck) callback to customise your response to a failed check.

Defaults to `true`.
