# Bot or tab package format (new change rolling out)

Your manifest file must be named `manifest.json` and be at the top level of the zip package

The current manifest schema is [here](../schema.md) but amended per the below.  

Your manifest for Preview must contain the following amended information:
| Field  | Required   | Description   | 
|-------------------------------|---|----------------------------------------------------------------------------|
|	id	|	*	|	Unique GUID.  For a bot, you can re-use your Bot Framework GUID.  For a tab, you can can [generate a new GUID](https://guidgenerator.com/)	|	
|	tabs	|		|	This section is required if you are providing a tab - for use in team channels - as part of your solution.	|	
| tabs->description | * | Both the short and full descriptions must accurately and adequately explain your experience. |
| tabs->icons | * | _See important notes below about icons_ |
|	bots	|		|	This section, previously marked _reserved for future use_ will be used for your optional bot submission.	|
|	bots->mri	|	*	|	This must be the Bot Framework ID for your registered bot solution.	|
|	bots->pinnedTabs	|	  | Your bot may optionally provide a static tab, shown in direct chat.	|
|	bots->pinnedTabs->id	|	*	|	Developer defined ID for the tab	|
|	bots->pinnedTabs->definitionId	|	*	|	Like an entity ID for a Teams tab, this can be used by you to identify the specific content on display.	|
|	bots->pinnedTabs->displayName	|	*	|	Name to show on the Tab UX	|
|	bots->pinnedTabs->url	|	*	|	The url for the content to show in the tab	|
|	bots->pinnedTabs->websiteUrl | * 	|	A fallback URL for the user to view in browser 	|

## Sample simple bot manifest
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


## Sample bot with pinned tab manifest

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

> **Note**: the pinnedTabs section is optional.  It's only required if your bot displays tabs alongside its 1:1 conversations with users.

## Sample tab manifest:

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

### Icons 

Icons are are only for tabs.  (For bots, your Bot Framework icon will be used.)
* Your manifest must specify 44x44 and 88x88 icons as files included inside your package (that are smaller than ~1.5KB) or via URLs on a publicly accessible server.
* The icons must be transparent PNGs, with a single white/light foreground color.
* The icon itself will render over the "accentColor" provided in the manifest.


## Test your manifest

Test your new manifest by zipping it up and following the [new method for side-loading](sideload.md).