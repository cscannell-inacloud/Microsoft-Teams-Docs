# Create the package for your Microsoft Teams tab or bot

To test your tab or bot, you need to create a sideload package and upload it to a team. The package is a zip file containing:

- A manifest file named "manifest.json", which specifies attributes of your tab or bot and points to required resources for your experience, such the location of its tabs configuration page or bot id.
- For tabs, image files, to be used as icons.  These must be transparent PNG files, with white or light-colored foreground in both small (44 by 44 pixels) and large (88 by 88 pixels) sizes.  (The accompanying background or 'accent color' is specified in the manifest.)

## Creating a manifest for your tab or bot

Your manifest file must be named "manifest.json" and be at the top level of the sideload package.

The current manifest schema is [here](schema.md).  Note that you must create a manifest for both tabs and bots.

> **Tip:** Specify the schema at the beginning of your manifest to enable IntelliSense or similar support from your code editor:
> 
> `"$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json",`


### Sample simple bot manifest
```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json",
  "manifestVersion": "0.4",
  "id": "%BOT-FRAMEWORK-APP-ID-HERE%",
  "version": "1.0",
  "developer": {
    "name": "%NAME-OF-PUBLISHER%",
    "websiteUrl": "https://website.com/",
    "privacyUrl": "https://website.com/privacy",
    "termsOfUseUrl": "https://website.com/app-tos"
  },
  "bots": [
    {
      "mri": "%BOT-FRAMEWORK-APP-ID-HERE%"
    }
  ],
  "needsIdentity": true
}
```

### Sample bot with pinned tab manifest

```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json", 
  "manifestVersion": "0.4",
  "version": "1.0",
  "developer": {
    "name": "%NAME-OF-PUBLISHER%",
    "websiteUrl": "https://website.com/",
    "privacyUrl": "https://website.com/privacy",
    "termsOfUseUrl": "https://website.com/app-tos"
  },
  "bots": [
    {
      "mri": "%BOT-FRAMEWORK-APP-ID-HERE%", 
      "pinnedTabs": [
        {
          "id": "%USER-DEFINED-ID%",  
          "definitionId": "%USER-DEFINED-ENTITY-ID%",
          "displayName": "%TAB-NAME%",
          "url": "http://taburl.com/teamsview",  
          "websiteUrl": "http://taburl.com/webview" 
        }
      ]
    }
  ],
  "needsIdentity": true
}
```

> **Note**: the pinnedTabs section is optional.  It's only required if your bot displays tabs alongside its 1:1 conversations with users. See [more](bottab.md).

### Sample tab manifest:

> Tip: [Download the example zip file](https://github.com/OfficeDev/microsoft-teams-sample-get-started/blob/master/package/MapsTab.zip) that contains the example manifest shown here.

```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json", 
  "manifestVersion": "0.4",
  "version": "1.0",
  "developer": {
    "name": "%NAME-OF-PUBLISHER%",
    "websiteUrl": "https://website.com/",
    "privacyUrl": "https://website.com/privacy",
    "termsOfUseUrl": "https://website.com/app-tos"
  },
  "tabs": [
    {
      "id": "%UNIQUE-GUID%",  
      "name": "%TAB-APP-NAME%",
      "description": {
        "short": "Short description of your Tab",
        "full": "Richer description of your Tab - 256 characters."
      },
      "icons": {
        "44": "%URL-OR-FILENAME-44x44px%", 
        "88": "%URL-OR-FILENAME-88x88px%", 
      },
      "accentColor": "%HEX-COLOR%",
      "configUrl": "https://taburl.com/config.html",
      "canUpdateConfig": true
    }
  ],
  "needsIdentity": true,
  "validDomains": [
     "*.taburl.com",
     "*.otherdomains.com"
  ]
}
```


## Create or reference icons for your tab

Icons are are only used for tabs.  For bots, your Bot Framework icon will be used.
* Your manifest must specify 44x44 and 88x88 icons as files included inside your package (that are smaller than ~1.5KB) or via URLs on a publicly accessible server.
* The icons must be transparent PNGs, with a single white/light foreground color.
* The icon itself will render over the "accentColor" provided in the manifest.

## Create and uploading your package to Microsoft Teams

Once you've created your manifest and image files, compress them into a zip file.  Your manifest must be at the **top** level of the zip.

Upload your zip file to a team to make your app available, via the sideloading process found [here](sideload.md).
