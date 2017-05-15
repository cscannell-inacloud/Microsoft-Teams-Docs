# Create a static tab

A static tab is a [content page](createcontentpage.md) that is declared directly in your manifest and - unlike a configurable tab - does not require a 'configuration page' to set it up.

Currently, you can add one or more static tabs to your app's "personal scope" experience, accessed via the [app bar](teamsapps.md) or alongside your app's bot conversation.

![Static Tabs example](images/tabs_in_bot.PNG)

## Creating tab content

Content pages in Teams, regardless of scope or type, should follow [these guidelines](createcontentpage.md).

## Adding your tabbed content to your app package

Define your static tab experience in the [manifest file](schema.md) in the `staticTabs` object.  

For more information on creating your app package, see [here](createpackage.md)

#### Manifest snippet - static tabs

```json
...
"staticTabs": [
  {
    "entityId": "TestAppAbout",
    "name": "About",
    "contentUrl": "https://teams-specific-webview.website.com/about",
    "websiteUrl": "http://fullwebsite.website.com/about",
    "scopes": [ "personal" ]
  },
  {
    "entityId": "TestAppMyTasks",
    "name": "My Tasks",
    "contentUrl": "https://teams-specific-webview.website.com/mytasks",
    "websiteUrl": "http://fullwebsite.website.com/mytasks",
    "scopes": [ "personal" ]
  }
]
...
```

<!-- TODO get this from sample app -->

The `staticTabs` object allows you to specify one or more tabs, up to 16, with the following required elements:

* `entityId` - this is a user-defined ID that uniquely identifies the tab.  This is analogous to the entityId used in [configurable tab deep linking](deeplinks.md).
* `name` - the name shown on the tab
* `contentUrl` - the content URL to show in the tab
* `websiteUrl` - the URL to the full chrome content to display in the default browser
* `scopes` - at this point, static tabs are only used in the `personal` context

## Add static tab URLs to validDomains

All URLs you add in static tabs must be referenced in the [`validDomains`](schema.md#validdomains) section of the manifest.  Failure to do so may result in a blank tab.  Please note that while you may use wildcards for subdomains, make sure you appropriately scope for only the content you control and expect in the tab experience.  E.g. yourapp.onmicrosoft.com - good; *.onmicrosoft.com - bad



