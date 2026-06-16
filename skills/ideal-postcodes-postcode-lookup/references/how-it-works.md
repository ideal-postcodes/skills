# How it Works

Postcode Lookup can be attached to any form with pre-existing address inputs and an empty container to render the Postcode Lookup UI.

When initialised it will:
1. Identify the context container defined by [`context`](https://postcode-lookup.ideal-postcodes.co.uk/classes/Controller.Controller-1.html#context)
2. Render the Postcode Lookup UI within the context container including:
    1. Postcode Lookup text field
    2. Postcode Lookup button
    3. Container to render address list
    4. Message container
3. Pipe address results to the fields defined in `outputFields`.
4. Perform a key check to determine whether it is usable. If the check fails, initialisation is aborted. Use the `onFailedCheck` callback to update your page for manual address entry.
