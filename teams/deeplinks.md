# Creating deep links to a Microsoft Teams (preview) tab

Deep links can be useful to direct users to specific pieces of content within your tab.

>**Note:** Deep links only work properly if the tab was configured using the v0.4 library and thus has an entity id. Deep links to tabs without entity ids will still navigate to the tab but not be able to provide the sub-entity id to the tab.

## Generating a deep link from a tab using the SDK

To generate a deep link to your tab which the user can copy and paste into a conversation or email, the SDK provides a function you can call from your tab. This call shows a modal dialog to the user in the Microsoft Teams client containing the deep link.

To invoke this dialog, call: `microsoftTeams.shareDeepLink({ subEntityId: <subEntityId>, subEntityLabel: <subEntityLabel>, subEntityWebUrl: <subEntityWebUrl> })`

The fields to provide are:
* `subEntityId` - a developer-defined id which points to the sub-entity within your tab which should be deep linked to.
* `subEntityLabel` - a label for the sub-entity to use for displaying the deep link.
* `subEntityWebUrl` - an optional field describing the fallback url to navigate the user to if the client does not support rendering the tab.

## Generating a deep link externally (from a bot or connector)

The format for a deep link is as follows:
`https://teams.microsoft.com/l/entity/<appId>/<entityId>?webUrl=<subEntityWebUrl>&label=<subEntityLabel>&context=<context>`

The query parameters are:
* `appId` - the id from your manifest
* `entityId` - the entity id you provided at tab configuration time
* `subEntityWebUrl` - an optional field describing the fallback url to navigate the user to if the client does not support rendering the tab.
* `subEntityLabel` - a label for the sub-entity to use for displaying the deep link.
* `context` - this is a JSON object containing the following fields.
    * `subEntityId` - a developer-defined id which points to the sub-entity within your tab which should be deep linked to.
    * `canvasUrl` - the url to load in the tab. Note that this field is currently required but not used as it is reserved for future use.
    * `channelId` - the Microsoft Teams channel id. Note that you can get this from your tab via a `microsoftTeams.getContext` call, so you can store this value in your backend when a tab is being configured.

> Example: `https://teams.microsoft.com/l/entity/com.example.test/123?webUrl=https%3A%2F%2Fexample.com&label=Example&context=%7B%22subEntityId%22%3A+%22456%22%2C%22canvasUrl%22%3A+%22https%3A%2F%2Fexample.com%22%2C%22channelId%22%3A+%22789%22%7D`

## Consuming a deep link from a tab

When navigating to a deep link, Microsoft Teams simply navigates to the tab and provides a mechanism via the SDK to retrieve the sub-entity id (if it exists).

The [`microsoftTeams.getContext`](jslibrary.md#getcontextcallback-context-contextcontext--void-void) call returns a context which will have the `subEntityId` field if the tab was navigated to via deep link.
