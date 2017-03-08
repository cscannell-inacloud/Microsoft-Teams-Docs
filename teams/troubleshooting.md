# Troubleshooting the Microsoft Teams (Preview) tabs

## Tabs disappear

During the developer preview, any tabs that you create, and upload to a team, will expire after 29 days.  Your tab will no longer be available in the tab gallery, and any tabs of this type that you, or the team, have added to channels will be removed from the team's channels.  

Tabs may also disappear during the roll out of a new process for side-loading tabs.  Please check [here](sideload.md) to determine if you need to use this process.

## Can't add bot as a member of a team

We have rolled out a change that means you can no longer load a bot by adding it as a team member, but by sideloading a bot definition package instead.  Please check [here](sideload.md) for more information on this process.

## Error while reading manifest.json

With the [new side-loading process](sideload.md), the transparent .png icons that you reference in the manifest must now be smaller than ~1.5KB.  Alternatively, you can reference the icon using a URL on a publicly accessible server, instead of a local path to icons inside the zip package: the size limit does not apply in this case.

Most other manifest errors will provide a hint at what specific field is missing or invalid. However, if the json file cannot be read as json at all, this generic error message is used.

Common reasons for manifest read errors:

* Invalid json. Use an IDE such as [Visual Studio Code](https://code.visualstudio.com) or [Visual Studio](https://www.visualstudio.com/vs/) that automatically validates the json syntax.
* Encoding issues. Use UTF-8 for the manifest.json file. Other encodings, specifically with the BOM, may not be readable.

## Another extension with same id "&lt;id&gt;" exists
If you're attempting to re-upload an updated package with the same id, use the 'Replace' icon at the end of the tab's table row rather than the 'Upload' button again.

If you're not re-uploading an updated package, ensure the id is unique.

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
