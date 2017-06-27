# Microsoft Teams manifest schema reference

>Note: for help on migrating your v0.4 manifest to v1.0, see [our migration guide](schemamigrate.md).

The Microsoft Teams manifest describes how the app integrates into the Microsoft Teams product. Your manifest must conform to the schema hosted at: [`https://statics.teams.microsoft.com/sdk/v1.0/manifest/MicrosoftTeams.schema.json`](https://statics.teams.microsoft.com/sdk/v1.0/manifest/MicrosoftTeams.schema.json).  The following is an example with all extensibility options:

>**Tip** Download our sample [Simple Bot Package](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/SimpleBotPackage.zip) or [Full App Package](https://github.com/OfficeDev/Microsoft-teams-docs/blob/master/teams/FullAppPackage.zip) to get started.

## Sample full schema
```json
{
  "$schema": "https://statics.teams.microsoft.com/sdk/v1.0/manifest/MicrosoftTeams.schema.json", 
  "manifestVersion": "1.0",
  "version": "1.0.0",
  "id": "%MICROSOFT-APP-ID%", 
  "packageName": "com.example.myapp",
  "developer": {
    "name": "Publisher Name",
    "websiteUrl": "https://website.com/",
    "privacyUrl": "https://website.com/privacy",
    "termsOfUseUrl": "https://website.com/app-tos"
  },
  "name": {
    "short": "Name of your app - 30 characters",
    "full": "Full name of app, if greater than 30"
  },
  "description": {
    "short": "Short description of your app",
    "full": "Full description of your app"
  },
  "icons": {
    "outline": "%FILENAME-20x20px%", 
    "color": "%FILENAME-96x96px" 
  },
  "accentColor": "%HEX-COLOR%",
  "configurableTabs": [
    {
      "configurationUrl": "https://taburl.com/config.html",
      "canUpdateConfiguration": true,
      "scopes": [ "team" ]
    }
  ],
  "staticTabs": [
    {
      "entityId": "idForPage",
      "name": "Display name of tab",
      "contentUrl": "https://teams-specific-webview.website.com",
      "websiteUrl": "http://fullwebsite.website.com",
      "scopes": [ "personal" ]
    }
  ],
  "bots": [
    {
      "botId": "%MICROSOFT-APP-ID-REGISTERED-WITH-BOT-FRAMEWORK%",
      "needsChannelSelector": "true",
      "isNotificationOnly": "false",
      "scopes": [ "team", "personal" ],
      "commandLists": [
        {
          "scopes": ["team"],
          "commands": [
            {
              "title": "Command 1",
              "description": "Description of Command 1"
            },
            {
              "title": "Command N",
              "description": "Description of Command N"
            }
          ]
        },
        {
          "scopes": ["personal"],
          "commands": [
            {
              "title": "Personal command 1",
              "description": "Description of Personal command 1"
            },
            {
              "title": "Personal command N",
              "description": "Description of Personal command N"
            }
          ]
        }
      ]
    }
  ],
  "connectors": [
    {
      "connectorId": "GUID-FROM-CONNECTOR-DEV-PORTAL%",
      "scopes": [ "team"]
    }
  ],
  "composeExtensions": [
    {
      "botId": "%MICROSOFT-APP-ID-REGISTERED-WITH-BOT-FRAMEWORK%",
      "scopes": ["team", "personal"],
      "commands": [
        {
          "id": "exampleCmd",
          "title": "Example Command",
          "description": "Comamand Description e.g. Search on the web",
          "initialRun": "true",
          "parameters": [
            {
              "name": "keyword",
              "title": "Search keywords",
              "description": "Enter the keywords to search for"
            }
          ]
        }
      ]
    }
  ],
  "permissions": [
    "identity",
    "messageTeamMembers"
  ],
  "validDomains": [
     "*.taburl.com",
     "*.otherdomains.com"
  ]
}
```



> **Tip** Specify the schema at the beginning of your manifest to enable IntelliSense or similar support from your code editor:
> 
> `"$schema": "https://statics.teams.microsoft.com/sdk/v1.0/manifest/MicrosoftTeams.schema.json",`


The schema defines the following properties:

## ManifestVersion 

**Required** - String


The version of the manifest schema this manifest is using.  Should be "1.0".

## version

**Required** - String

This is the version of the specific app. If you update something in your manifest, the version must be incremented as well. This way, when the new manifest is installed, it will overwrite the existing one and the user will get the new functionality. If this app was submitted to the store, the new manifest will have to be re-submitted and re-validated. Then, users of this app will get the new updated manifest automatically in a few hours, after it is approved.

If the app requested permissions change, users will be prompted to upgrade and re-consent to the app. 

This version string must follow the [semver](http://semver.org/) standard (MAJOR.MINOR.PATH).

## id

**Required** - Microsoft app ID

This is the unique Microsoft-generated identifier for this app.  If you have registered a bot via the Microsoft Bot Framework, or your tab's web app already signs in with Microsoft, then you should already have such an ID and should enter it here. Otherwise, you should generate a new ID at the [Microsoft App registration portal](https://apps.dev.microsoft.com), enter it here, and then re-use it if/when you [add a bot](botscreate.md).

## packageName

**Required** - String

This is a unique identifier for this app in reverse domain notation. E.g: com.example.myapp

## developer

**Required**

The developer element specifies information about your company.  For Store-submitted apps, these values must match the information in your Store entry.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`name`|String|32|✔|The display name for the developer.|
|`websiteUrl`|String|2048|✔|The URL to the developer's website.  This link should take users to your company or product-specific landing page.|
|`privacyUrl`|String|2048|✔|The URL to the developer's privacy policy.|
|`termsOfUseUrl`|String|2048|✔|The URL to the developer's terms of use.|


## name

**Required**

This is the name of your app experience, displayed to users in the Teams experience.  For Store-submitted apps, these values must match the information in your Store entry.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`short`|String|30|✔|The short display name for the app.|
|`full`|String|100||The full name of the app, used if the full app name exceeds 30 characters.|


## description

**Required**

These values describe your app to users.  For Store-submitted apps, these values must match the information in your Store entry.

Please note that you should make your description accurately describes your experience, and provided the right information to help potential new customers understand what your experience does.  You should also note, in the full description, if an external account is required for use.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`short`|String|80|✔|A short description of your app experience, used when space is limited.|
|`full`|String|4000*|✔|The full description of your app.|

>**NOTE** - There is currently an issue with >256 character full descriptions.  You may use a longer description in your Seller Dashboard app submission.



## icons

**Required**

Icons are used within the Teams app, and must be included as part of the sideload package.  See [here](createpackage.md#icons) for more information.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`outline`|String|2048|✔|A relative file path to a transparent 20x20 PNG outline icon.|
|`color`|String|2048|✔|A relative file path to a full color 96x96 PNG icon.|

## accentColor

**Required**

A color to use in conjunction with and as a background for your outline icons.

The value must be a valid HTML color code starting with '#', for example `#4464ee`.

## configurableTabs

*Optional*

The configurableTabs block is used in cases where your app experience has a team channel tab experience that requires extra configuration before it is added.  Configurable tabs are only supported in the teams scope, and currently only one per app is supported.

The object is an array with all elements of the type `object`.  This block is only required for solutions that provide a configurable channel Tab solution.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`configurationUrl`|String|2048|✔|The URL to use when configuring the tab.|
|`canUpdateConfiguration`|Boolean|||A value indicating whether an instance of the tab's configuration can be updated by the user after creation.  Default: `true`|
|`scopes`|Array of enum|1|✔|Currently, configurable tabs only support the `team` scope, which means it can only be provisioned to a channel.|


## staticTabs

*Optional*

The staticTabs block defines a set of tabs that may be "pinned" by default, without the user adding the manually.  Static tabs declared in `personal` scope are always pinned to the app's personal experience.  Static tabs declared in the `team` scope are currently not supported. 

The object is an array (max: 16) with all elements of the type `object`.  This block is only required for solutions that provide a static tab solution.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`entityId`|String|64|✔|A unique identifier for the entity which the tab displays.|
|`name`|String|128|✔|The display name of the tab in the channel interface.|
|`contentUrl`|String|2048|✔|The URL which points to the entity UI to be displayed in the Teams canvas.  Must be HTTPS.|
|`websiteUrl`|String|2048||The URL to point at if a user opts to view in a browser.|
|`scopes`|Array of enum|1|✔|Currently, static tabs only support the `personal` scope, which means it can only be provisioned in as part of the personal experience.|


## bots

*Optional*

The bots block defines a bot solution, along with additional optional information like default command properties.

The object is an array (max: 1 -- currently only one bot is allowed per app) with all elements of the type `object`.  This block is only required for solutions that provide a Bot experience.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`botId`|String|64 characters|✔|The unique Microsoft app ID for the bot as registered with the Bot Framework. This may well be the same as the overall [app ID](#id).|
|`needsChannelSelector`|Boolean|||This value describes whether or not the bot utilizes a user hint to add the bot to a specific channel. Default: `false`|
|`isNotificationOnly`|Boolean|||A flag which indicates whether a bot is a one-way notification only bot, as opposed to a conversational bot. Default: `false`|
|`scopes`|Array of enum|2|✔|Specifies whether the bot offers an experience in the context of a channel in a `team`, or an experience scoped to an individual user alone (`personal`). These options are non-exclusive.|

### bots: commandLists

>[Public Developer Preview only](publicpreview.md)

You can provide an optional list of commands that your bot can recommend to users.  The object is an array (max: 2) with all elements of type `object` - you must define a separate command list for each scope that your bot supports.  See [here](botmenu.md) for more information.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`items.properties`|array of enum|2|✔|Specifies the scope for which the command list if valid.|
|`items.commands`|array of objects|10|✔|An array of commands the bot supports:<br>`title`: the bot command name (string, 32)<br>`description`: a simple description or example of the command syntax and its argumenst (string, 128)|


## connectors


*Optional*

>[Public Developer Preview only](publicpreview.md)

The connectors block defines an Office365 connector for the app.  

The object is an array (max:1) with all elements of type `object`.  This block is only required for solutions that provide a connector.
     
|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`connectorId`|String|64|✔|A unique identifier for the connector that matches its ID in the Connectors Developer Portal.|
|`scopes`|Array of enum|1|✔|Specifies whether the connector offers an experience in the context of a channel in a `team`, or an experience scoped to an individual user alone (`personal`). Currently, only the `team` scope is supported.|

## composeExtensions

*Optional*

>[Public Developer Preview only](publicpreview.md)

The composeExtension block defines a compose extension for the app.

The object is an array (max:1) with all elements of type `object`.  This block is only required for solutions that provide a compose extension.

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`botId`|String|64|✔|The unique Microsoft app ID for the bot that backs the compose extension, as registered with the Bot Framework. This may well be the same as the overall [app ID](#id).|
|`scopes`|Array of enum|2|✔|Specifies whether the bot offers an experience in the context of a channel in a `team`, or an experience scoped to an individual user alone (`personal`). These options are non-exclusive.|
|`commands`|Array of object|1|✔|Array of commands the compose extension supports|

### composeExtensions.commands
Your compose extension should declare one or more commands. Each command appears in Microsoft Teams as a potential interaction from the UI-based entry point.

Each command item is an object with the following structure:

|Name| Type| Maximum Size | Required | Description|
|---|---|---|---|---|
|`id`|String|64|✔|The ID for the command|
|`title`|String|32|✔|The user friendly command name|
|`description`|String|128||The description that appears to users to indicate the purpose of this command|
|`initialRun`|Boolean|||A boolean value that indicates if the command should be run initially with no parameters.  Default: `false`|
|`parameters`|Array of Objects|5|✔|The list of parameters the command takes.  Min:1, Max: 5|
|`parameter.name`|String|64|✔|The name of the parameter as it appears in the client.  This is included in the user request.|
|`parameter.title`|String|32|✔|User-friendly title for the parameter.|
|`parameter.description`|String|128||User-friendly string that describes this parameter’s purpose.|


## permissions

*Optional*

The permissions array specifics which permissions the extensions are requesting, which lets end users know how the extension will perform.  The following options are non-exclusive:

* `identity` - requires user identity information
* `messageTeamMembers` - requires the permission to send direct messages to team members


## validDomains


*Optional*, **Required** for apps with Tabs.

A list of valid domains from which the extension expects to load any content. Domain listings can include wildcards, for example `*.example.com`. If your tab configuration or content UI needs to navigate to any other domain besides the one use for tab configuration, that domain must be specified here.

>**Note: You may not add domains outside your control, either directly, or via wildcards.  E.g. youapp.onmicrosoft.com is valid, but *.onmicrosoft.com would not be valid.

The object is an array with all elements of the type `string`.
