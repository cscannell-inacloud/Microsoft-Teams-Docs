# Microsoft Teams (preview) tab manifest schema reference

Your [manifest](createpackage.md) must conform to the schema hosted at: `https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json` .

> **Tip** Specify the schema at the beginning of your manifest to enable IntelliSense or similar support from your code editor:
> 
> `"$schema": "https://statics.teams.microsoft.com/sdk/v0.4/manifest/MicrosoftTeams.schema.json",`

The schema defines the following properties:

## `manifestVersion` (string, required)

The version of the manifest schema this manifest is using.  Should be "0.4"

## `version` (string, required)

The app version. Changes to the app should cause a version change. This version string must follow the semver standard.

## `id` (string)

A unique identifier for this app. The id must be a GUID.  For a Bot, you may use the BotFramework Id.  For a tab, you can use this [online tool](https://guidgenerator.com/) to generate a GUID, or use one of your choosing.

## `developer` (object, required)

Properties of the `developer` object:

### `name` (string, required)

The display name for the developer.

### `websiteUrl` (string, required)

The url to the developer's website.

### `privacyUrl` (string, required)

The url to the developer's privacy policy.

### `termsOfUseUrl` (string, required)

The url to the developer's terms of use.

## `tabs` (array)

The object is an array with all elements of the type `object`.  This block is only required for solutions that provide a configurable channel Tab solution.

The array object has the following properties:

### `id` (string, required)

A unique identifier for this extension. The id must be a GUID.  You may use the same ID as at the parent level.

### `name` (string, required)

The display name of the extension.

### `description` (object, required)

The description information is displayed to users at various points in the UX.  The description you provide must adequately and accurately explain your experience, to new users or existing.

Properties of the `description` object:

#### `short` (string, required)

A short description of the extension used when space is limited. Maximum length is 80 characters.

#### `full` (string, required)

The full description of the extension. Maximum length is 256 characters.

### `icons` (object, required)

Each icon image file must be a transparent PNG, with a white or light-colored background.  Note that you may use a URL to a hosted version of your icons.  If you choose to include an icon in your submission package, please make sure you follow the [guidelines](createpackage.md).

Properties of the `icons` object:

#### `44` (string, required)

An icon for the extension sized to 44x44.

#### `88` (string, required)

An icon for the extension sized to 88x88.

### `accentColor` (string, required)

A color to use in conjunction with and as a background for the tab's icons. The value must be a valid HTML color code starting with '#', for example `#4464ee`.

### `configUrl` (string, required)

The url to use when configuring the extension.

### `canUpdateConfig` (boolean)

A value indicating whether an instance of the extension's config can be updated by the user after creation.

Default: `true`

## `bots` (array)

The object is an array with all elements of the type `object`.  This block is only required for solutions that provide a Bot experience.

The array object has the following properties:

### `mri` (string, required)

This must be the Bot Framework ID for your registered bot.

### `pinnedTabs` (object)

Your bot may optionally provide a static tab, shown alongside the bot's direct chat view.

#### `id` (string, required)

Developer defined ID for the tab.

#### `definitionId`	(string, required)

Like an entity ID for a Teams tab, this can be used by you to identify the specific content on display.

#### `displayName` (string, required)	

Name to show on the Tab UX.

#### `url` (string, required)	

The url for the content to show in the tab

#### `websiteUrl` (string, required)

A fallback URL for the user to view in browser

## `needsIdentity` (boolean)

A value indicating whether the extension is requesting access to identity information

## `validDomains` (array)

A list of valid domains from which the extension expects to load any content. Domain listings can include wildcards, for example `*.example.com`. If your tab configuration or content UI needs to navigate to any other domain besides the one use for tab configuration, that domain must be specified here.

The object is an array with all elements of the type `string`.
