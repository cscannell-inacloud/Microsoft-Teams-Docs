# Troubleshooting Microsoft Teams tabs and bots

## Tabs disappear

During the developer preview, any tabs that you created, and uploaded to a team, may have expired after 29 days.  This expiration issue has since been addressed, but you will have to [sideload](sideload.md) the tab again for it to appear in your Tab Gallery.  

Tabs may also disappear during the roll out of a new process for side-loading tabs.  Please check [here](sideload.md) to determine if you need to use this process.

## Can't add bots in general

Bots must be enabled by the O365 Tenant admin for them to be loaded by end users.  Note that in some cases, the O365 Tenant may have multiple SKUs associated with it, and for bots to work in any, they must be enabled in all SKUs.  See [here](https://msdn.microsoft.com/en-us/microsoft-teams/setup#enable-sideloading-of-bots-and-tabs) for more information.


## Can't add bot as a member of a team

We have rolled out a change that means you can no longer load a bot by adding it as a team member, but by sideloading a bot definition package instead.  Please check [here](sideload.md) for more information on this process.


## Error while reading manifest.json

With the [new side-loading process](sideload.md), the transparent .png icons that you reference in the manifest must now be smaller than ~1.5KB.  Alternatively, you can reference the icon using a URL on a publicly accessible server, instead of a local path to icons inside the zip package: the size limit does not apply in this case.

Most other manifest errors will provide a hint at what specific field is missing or invalid. However, if the json file cannot be read as json at all, this generic error message is used.

Common reasons for manifest read errors:

* Invalid json. Use an IDE such as [Visual Studio Code](https://code.visualstudio.com) or [Visual Studio](https://www.visualstudio.com/vs/) that automatically validates the json syntax.
* Encoding issues. Use UTF-8 for the manifest.json file. Other encodings, specifically with the BOM, may not be readable.

## The Save button isn't enabled on the settings dialog
Be sure to call `microsoftTeams.setValidityState(true)` once the user has input or selected all required data on your settings page to enable the save button.

## After selecting the Save button, the tab settings cannot be saved.
When adding a tab, if you click the save buttons but are presented with an error message indicating the settings cannot be saved, the problem could be one of two classes of issues.

### The save success message was never recieved
If a save handler was registered using `microsoftTeams.settings.registerOnSaveHandler(handler)`, the callback must call `saveEvent.notifySuccess()`. If the callback doesn't call this within 30 seconds or calls `saveEvent.notifyFailure(reason)` instead, this error will be shown.

If no save handler was registered, the `saveEvent.notifySuccess()` call is automatically made immediately upon the user selecting the save button.

### The provided settings were invalid
The other reason the settings may not be saved is if the call to `microsoftTeams.setSettings(settings)` provided an invalid settings object, or the call wasn't made at all.

Common problems with the settings object:

* `settings.entityID` is missing. This field is required.
* `settings.contentUrl` is missing. This field is required.
* `settings.contentUrl` or the optional `settings.removeUrl`, or `settings.websiteUrl` are provided but not valid. The urls must be https urls and also must either be the same domain as the settings page or specified in the manifest's `validDomains` list.
