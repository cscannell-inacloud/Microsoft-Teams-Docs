# Using the Activity Feed

>Note: Activity feed support is available in [Public Developer Preview](publicpreview.md) only.  Additionally, many features in this document are under construction, and as such, subject to change.


The Activity Feed in Microsoft Teams is a single inbox for all activity related to your activity across Teams.  It aggregates important content from:
* Teams/channels
* Chats
* Apps such as Files, Planner and your Teams app

You can now create preview cards to summary app activity, and even surface quick actions such as approve/decline, mark as done, or any other task relevant for your experience.  Clicking on the card can also deep link the user straight to the message or object that triggered the notification, so you can see the full content of the activity.

## Sending content to the Activity Feed

Activity feed notification leverages your bot, built for Microsoft Teams using the Bot Framework.  You can flag specific messages to generate notifications which appear in the Activity Feed. This allows generating higher levels of engagement by creating alerts on web/desktop and mobile apps.

In addition to simply appearing in the feed, your app can also encode a deep link URL to an entity, such as your app’s tab. This drives user engagement to your app’s tab by allowing one-click navigation to that tab’s content.

### Rest API

For a message to be eligible to be included in the feed, simply mark an existing bot message with a special property, which indicates it should generate a notification:

```json
"channelData": {
  "notification": {
    "alert": true
  }
}
```

#### Request example

```json
POST /v3/conversations/a%3A1pL6i0oY3C0K8oAj8/activities/1493070356924
{
  "type": "message",
  "timestamp": "2017-04-24T21:46:00.9663655Z",
  "serviceUrl": "https://callback.com",
  "channelId": "msteams",
  "from": {
    "id": "28:e4fda94a-4b80-40eb-9bf0-6314491bc793",
    "name": "The bot"
  },
  "conversation": {
    "id": "a:1pL6i0oY3C0K8oAj8"
  },
  "recipient": {
    "id": "29:1rsVJmSSFMScF0YFyCXpvNWlo",
    "name": "User"
  },
  "text": "Test feeds.",
  "channelData": {
    "notification": {
      "alert": true
    }
  },
  "replyToId": "1493070356924"
}
```

## Deep linking

To deep link to content within a tab, your message must include an attachment with a tap action. This tap action should be of type `OpenUrl` and have a value that follows the [Microsoft Teams deep linking format](deeplinks.md).

Note: if the deep link does not follow the Teams format, then clicking on the notification in the Feed will navigate the user first to the chat with the bot. From there, the user can engage the attachment’s tap action to navigate to an external website.


