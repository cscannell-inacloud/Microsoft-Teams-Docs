# Manifest guidance (early preview)

Your manifest file must be named `manifest.json` and be at the top level of the submission package (zip file)

The current manifest schema is [here](../schema.md) but amended per the below.  

Your manifest for Preview must contain the following amended information:
| Field  | Required   | Description   | 
|-------------------------------|---|----------------------------------------------------------------------------|
|	id	|	*	|	Unique identifier for the experience.  Note: Must be a guid for Preview.  [GUID generator](https://guidgenerator.com/)	|	
|	tabs	|		|	This section is required if you are providing a tab - for use in team channels - as part of your solution.	|	
| tabs->description | * | Both the short and full descriptions must accurately and adequately explain your experience. |
| tabs->icons | * | These must either reference icons in your package that are smaller than ~1.5KB or a URL on a publicly accessible server. |
|	bots	|		|	This section, previously marked _reserved for future use_ will be used for your optional bot submission.	|
|	bots->mri	|	*	|	This must be the Bot Framework ID for your registered bot solution.	|
| _Note: The following will be supported by launch._ | | _Included so you can craft your manifest in preparation:_ | 
|	bots->pinnedTabs	|	  | Your bot may optionally provide a static tab, shown in direct chat.	|
|	bots->pinnedTabs->id	|	*	|	User defined ID for the tab	|
|	bots->pinnedTabs->definitionId	|	*	|	Like an entity ID for a Teams tab, this can be used by you to identify the specific content on display.	|
|	bots->pinnedTabs->displayName	|	*	|	Name to show on the Tab UX	|
|	bots->pinnedTabs->url	|	*	|	The url for the content to show in the tab	|
|	bots->pinnedTabs->websiteUrl | * 	|	A fallback URL for the user to view in browser 	|

## Sample bot manifest

```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json", 
  "manifestVersion": "0.4",
  "version": "1.0",
  "developer": {
    "name": "Yuri Dogandjiev",
    "websiteUrl": "https://todolistsample.azurewebsites.net",
    "privacyUrl": "https://todolistsample.azurewebsites.net/privacy",
    "termsOfUseUrl": "https://todolistsample.azurewebsites.net/termsofuse"
  },
  "bots": [
    {
      "mri": "9a346a73-fe93-484c-8236-6bc7203830e1", 
      "pinnedTabs": [
        {
          "id": "personal_todolist",  
          "definitionId": "com.skype.teams.samples.todolist",
          "displayName": "Personal list",
          "url": "https://todolistsample.azurewebsites.net?version=0.4",  
          "websiteUrl": "https://todolistsample.azurewebsites.net" 
        }
      ]
    }
  ]
}
```

## Sample tab manifest:

```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json", 
  "manifestVersion": "0.4",
  "version": "1.0",
  "developer": {
    "name": "Yuri Dogandjiev",
    "websiteUrl": "https://todolistsample.azurewebsites.net",
    "privacyUrl": "https://todolistsample.azurewebsites.net/privacy",
    "termsOfUseUrl": "https://todolistsample.azurewebsites.net/termsofuse"
  },
  "tabs": [
    {
      "id": "56E1A16C-DDB4-46C0-B4B1-FC634ED86DDD",  
      "name": "ToDo Sample App",
      "description": {
        "short": "Create Simple Personal ToDo lists",
        "full": "Create Simple Personal ToDo lists in Microsoft Teams.  Sign in with your favorite service, and start creating your own personal ToDo lists."
      },
      "icons": {
        "44": "https://todolistsample.azurewebsites.net/icon44x44.png", 
        "88": "https://todolistsample.azurewebsites.net/icon88x88.png"
      },
      "accentColor": "#ff6a00",
      "configUrl": "https://todolistsample.azurewebsites.net/config?version=0.4",
      "canUpdateConfig": true
    }
  ],
  "needsIdentity": true,
  "validDomains": [
     "*.bing.com",
     "*.google.com"
  ]
}
```

## Test your manifest

Test your new manifest by zipping it up and following the [new method for side-loading](sideload.md).