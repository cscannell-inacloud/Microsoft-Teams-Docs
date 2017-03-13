# Create a tab for your bot

> New feature

You may have more information about your bot or supported product than can be handled in a bot's conversation flow.  Or there might be persistant information that may make sense to have always available to the user, regardless of their bot conversation state.  With Microsoft Team's rich web-canvas tabs, you can now keep that information alongside your bot in the same context your user interacts with it.

Like our Team help bot T-Bot, you can add one or more tabs in your bot conversation channel.  This is simply web content that you host, relevant for your experience, like FAQs, help or other general information for users of your experience.

![Tabs in a bot](images\tabs_in_bot.png)

## Creating tab content

Since this is non-interactive content, there are no changes required for content you pin in bot tabs.  Simply check to make sure your site is responsive and can be displayed in an iframe.

Should you wish to display channel-specific information about your experience, please check out our [configurable tabs information](tabs.md).

## Adding your tabbed content to your bot package

Define your bot's tab experience in the bot's [manifest file](schema.md) in the bots section.  For more information on creating your manifest and schema, see [here](createpackage.md)

#### Manifest snippet - bots with a pinned tab

```json
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
]
```

The `pinnedTabs` object allows you to specify one or more tabs, with the following required elements:

* `id` - this is a user-defined ID (GUID or otherwise), that uniquely identifies the tab
* `definitionId` - another user-defined ID, analogous to the entity id used in configurable tab deep linking.
* `displayName` - the name shown on the tab
* `url` - the content url to show in the tab
* `websiteUrl` - a fallback url to display in the default browser



